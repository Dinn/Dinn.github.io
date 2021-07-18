---
title: 'React와 Canvas를 이용한 간단 이미지 편집 도구'
date: 2021-07-18
last_modified_at: 2021-07-18T22:16:56

category:
  - Development

tags:
  - React
  - Canvas
---

얼마전 회사에서 내부 운영툴에 사용하기 위한 이미지 편집 도구를 개발했습니다.

거창한 건 아니고 사용자로부터 등록된 이미지를 검수하기 위한 편집기였는데요, 기능도 간단하여 아래 두 가지 기능 정도만 포함하고 있습니다.
1. 이미지 회전
1. 이미지 블러

등록된 이미지의 위아래 방향이 맞지 않거나, 이미지에 민감 정보가 포함된 경우 이를 보정하려는 요구사항이 반영됐기 때문입니다.

아무튼, 개발 경험이 적은 저로서는 이미지를 다루는 것 자체가 생소했기에 새로운 도전이었습니다.
역시나 개발하며 많은 고민과 삽질이 있었고 이를 본 글을 통해 정리하고자 합니다.

참고로 이미지 편집 도구는 [TypeScript](https://www.typescriptlang.org/), [React](https://ko.reactjs.org/), [Next.js](https://nextjs.org/), [Canvas API](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API)를 사용하여 구현했습니다.
TS, React와 Next는 회사 프로젝트 개발 환경에 맞추기 위해 선택했습니다.
또 이미지 조작 혹은 이미지 편집과 관련한 라이브러리들이 몇 있긴 했지만 결국 Canvas API를 선택했습니다.
구현해야 하는 기능에 딱 맞춘 이미지 편집 라이브러리를 찾기가 매우 어려웠기 때문입니다.
vanilla로 하지 않고 Canvas API를 기반으로 독자적인 api를 제공하는 라이브러리<sub>(konva.js 같은...)</sub>도 있었지만, Canvas API를 직접 사용하는 거에 비해 진입 장벽이나 api의 복잡도가 그렇게 개선되지 않았기에 채택하지 않았습니다.

기능 구현 이후, 제 나름대로 다시 복기하며 데모를 만들어 봤습니다. 코드는 [https://github.com/Dinn/simple-image-editor](https://github.com/Dinn/simple-image-editor) 에 있습니다.
![demo](/assets/images/2021-05-28-simple-image-editor/image-editor-demo.gif)



## Canvas with React
본격적인 기능 구현을 하기 전에 우선 React에서 Canvas API를 사용하기 위한 기본적인 방법부터 알아보겠습니다.

Canvas는 vanilla JS 상에서 DOM 조작을 통한 그래픽 작업을 지원하는 api 입니다. 
이러한 특징은 선언적이고, props / state update 등과 같은 특정 조건에 의해 rendering이 이뤄지는 react에선 다소 어색한 접근입니다.

그렇기에 React에서 Canvas를 랜더링하기 위해선
1. canvas의 HTML element reference를 가져오기
1. 컴포넌트의 re-rendering을 일으켜 변경된 화면을 보여주기

이 두 가지를 반드시 고려해야 합니다.

### 1. canvas element 참조
canvas의 HTML element reference는 react의 [`useRef`](https://ko.reactjs.org/docs/hooks-reference.html#useref) 훅을 이용하여 가져올 수 있습니다.

```ts
import { useRef } from 'react';

export default function ImageEditor() {
  const canvasRef = useRef<HTMLCanvasElement>(null);

  return <canvas ref={canvasRef} />;
}
```


### 2. rendering canvas
하지만 canvas element의 reference를 가져왔더라도 아래와 같은 코드로는 `canvas`에 접근할 수 없습니다.

```ts
import { useRef } from 'react';

export default function ImageEditor() {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const canvas = canvasRef.current;
  const context = canvas.getContext('2d');

  return <canvas ref={canvasRef} />;
}
```

`ImageEditor` 컴포넌트가 mount되기 전에 `canvas`에 접근하고 있기 때문입니다. 이 문제는 `useEffect` 훅을 사용하여 mount 이후에 `canvasRef`를 접근하는 것으로 해결할 수 있습니다.

```diff
+ import { useRef, useEffect } from 'react';

  export default function ImageEditor() {
    const canvasRef = useRef<HTMLCanvasElement>(null);
  
+   useEffect(() => {
+     const canvas = canvasRef.current;
+     const context = canvas?.getContext('2d');
+
+     context?.fillRect(0, 0, 50, 50);
+   });

    return <canvas ref={canvasRef} />;
  }
```
위의 코드는 `useEffect`에 의해 매 rendering마다 `x: 0, y: 0, width: 50, height: 50`인 사각형을 그릴 것입니다.

`useEffect`의 callback 함수는 dependencies가 변할 때마다 재호출된다는 점을 이용하여 특정 값이 변할 때 canvas에 변경 사항을 반영하거나 그대로 있도록 제어할 수도 있겠습니다.
뒤이어 나올 내용들도 이같은 성질을 이용하고 있고요.



## Draw Image on Canvas
`canvas`를 마련했다면 이제 그 위에 이미지를 그려보겠습니다.

이미지는 직접 생성한 `HTMLImageElement` 객체를 통해 나타낼 것입니다.

또한 `HTMLImageElement` 객체에 접근하거나 `drawImage()`를 호출하는 등의 작업이 이미지가 로드된 이후에 이뤄지는 것을 보장하도록 이미지 관련 작업을 `onload` 이벤트에 바인딩하는 것이 중요합니다.

```diff
  import { useRef, useEffect } from 'react';
  
+ interface Props {
+   source: string
+ }

  export default function ImageEditor({ source }: Props) {
    const canvasRef = useRef<HTMLCanvasElement>(null);
    
    useEffect(() => {
      const canvas = canvasRef.current;
      const context = canvas?.getContext('2d');
  
+     const image = new Image();
+     image.src = source;
+     image.onload = () => {
+       context?.drawImage(image, 0, 0, 500, 500);
+     }
    });
  
+   return <canvas ref={canvasRef} width={500} height={500} />;
  }
```

![500 * 500 이미지](/assets/images/2021-05-28-simple-image-editor/absolute-size.png)

이미지의 소스는 `ImageEditor` 컴포넌트를 사용할 때 부모로부터 받아오도록 props 처리했습니다.

이 코드는 `onload` 이벤트가 발생할 시, 그러니까 이미지 로드가 완료된 이후 `context.drawImage`를 호출합니다.
이로 인해 브라우저의 좌측 최상단부터 시작해서 500 * 500 크기의 이미지를 그리게 됩니다.

이미지를 그릴 수 있게 된 것은 만족스럽지만, 가로/세로 비율이 서로 다른 이미지가 들어왔을 때 각자의 비율대로 그려줄 필요성이 느껴집니다.
대신 고해상도의 이미지를 그대로 보여주다보면 이미지가 너무 큰 경우가 있으므로, canvas 크기의 최대값을 정하여 그 안에서 비율을 유지하도록 해보겠습니다.

비율 조정 단계는 다음과 같습니다.
1. canvas의 가로/세로 최대값(`MAX_CANVAS_WIDTH`, `MAX_CANVAS_HEIGHT`)을 설정
1. 가로가 긴 이미지이라면 가로의 길이를 `MAX_CANVAS_WIDTH`에, 세로로 긴 이미지라면 세로의 길이를 `MAX_CANVAS_HEIGHT`에 맞춤
1. 나머지 길이를 이미지의 비율대로 확대/축소

설정한 단계대로 이미지를 다시 그려보겠습니다.

```diff
  import { useRef, useEffect } from 'react';
  
+ // #1
+ const MAX_CANVAS_WIDTH = 800;
+ const MAX_CANVAS_HEIGHT = 600;

  interface Props {
    source: string
  }
  
+ export default function ImageEditor({ source }: Props) {
    const canvasRef = useRef<HTMLCanvasElement>(null);
    
    useEffect(() => {
      const canvas = canvasRef.current;
      const context = canvas?.getContext('2d');
  
      const image = new Image();
      image.src = source;
      image.onload = () => {
+       // #2, #3
+       const width = image.width > image.height 
+         ? MAX_CANVAS_WIDTH 
+         : (image.width * MAX_CANVAS_HEIGHT) / image.height;
 
+       const height = image.width > image.height 
+         ? (image.height * MAX_CANVAS_WIDTH) / image.width 
+         : MAX_CANVAS_HEIGHT;
 
+       context?.drawImage(image, 0, 0, width, height);
      };
    });
  
+   return <canvas ref={canvasRef} width={MAX_CANVAS_WIDTH} height={MAX_CANVAS_HEIGHT}/>;
  }
```

![비율 보정 이미지](/assets/images/2021-05-28-simple-image-editor/relative-size.png)



## Rotate Image
이미지의 회전은 `context.rotate` 메소드를 통해 구현할 수 있습니다.

정확히는 `context.rotate`를 호출하여 canvas를 회전시키고, 회전한 canvas 위에 이미지를 그려 회전된 이미지를 얻는 것이지만 말입니다.
그런데 회전 시 몇 가지 주의사항이 있습니다.

우선 `context.rotate`메소드는 radian 단위를 사용한다는 점입니다. 
이를 우리가 익숙한 degree<sub>(90도, 180도, ...)</sub>로 변환하기 위해선 `radian = 원주율 / 180 * degree` 공식을 이용해 

```js
context.rotate((Math.PI / 180) * degrees)
```

와 같이 호출해야 합니다.

또한 canvas의 회전 방식도 주의해야 합니다.
`context.rotate`은 원점을 중심으로 주어진 각도만큼 원을 그리며 이동하는 방식입니다.

<div>
  <img alt='wrong rotation' src='/assets/images/2021-05-28-simple-image-editor/wrong-rotation.jpeg' width='49%' />
  <img alt='correct rotation' src='/assets/images/2021-05-28-simple-image-editor/correct-rotation.jpeg' width='49%' />
</div>

다시 말해, 왼쪽이 아닌 오른쪽과 같은 방식으로 회전하며, 도형이 아니라 canvas 자체가 회전하는 방식인 겁니다. 그래서 만약 1번 구획에서 90도 회전한다면 canvas는 2번 구획으로 위 이미지 처럼 이동하고, 이 상태로 canvas에 도형을 그리게 된다면 2번 구획에서 원점을 기준으로 x, y값을 계산하여 그리게 됩니다.

그런데, 사용자가 보는 웹 브라우저 상의 화면은 1번 구획이기 때문에 90도 회전을 완료했다면 canvas는 우리가 보는 화면을 벗어나게 됩니다. 웹 브라우저 상에서 회전한 도형을 보고 싶다면 canvas 상의 도형을 x축을 기준으로 원본 canvas의 `height` 만큼 평행 이동 시켜야 하는 것이죠.

실제 코드에선 회전하는 각도를 상태로 선언하고 `useEffect`의 dependency로 두었습니다. 덕분에 회전 각도가 변경될 때마다 re-rendering이 발생하고, 그때마다 canvas가 회전한 정도를 계산하여 회전된 이미지를 그립니다.

```diff
+ import { useRef, useEffect, useState } from 'react'

  const MAX_CANVAS_WIDTH = 800;
  const MAX_CANVAS_HEIGHT = 600;

  interface Props {
    source: string
  }

  export default function ImageEditor({ source }: Props) {
    const canvasRef = useRef<HTMLCanvasElement>(null);
+   const [rotationAngle, setRotationAngle] = useState<number>(0);
    
    useEffect(() => {
      const canvas = canvasRef.current;
      const context = canvas?.getContext('2d');

      const image = new Image();
      image.src = source;
      image.onload = () => {
+       // #1
+       const canvasWidth = rotationAngle % 180 ? MAX_CANVAS_HEIGHT : MAX_CANVAS_WIDTH
+       const canvasHeight = rotationAngle % 180 ? MAX_CANVAS_WIDTH : MAX_CANVAS_HEIGHT

        const width = image.width > image.height 
+         ? canvasWidth 
+         : (image.width * canvasHeight) / image.height;
        
        const height = image.width > image.height 
+         ? (image.height * canvasWidth) / image.width 
+         : canvasHeight;
  
+       // #2 
+       const x = -Math.floor(rotationAngle / 180) * width;
+       const y = -Math.floor(((rotationAngle + 90) % 360) / 180) * height;
        
+       // #3
+       if(canvas) {
+         canvas.width = rotationAngle % 180 ? height : width
+         canvas.height = rotationAngle % 180 ? width : height
+       }
        
+       // #4
+       context?.rotate((Math.PI /  180) * rotationAngle);
        
        context?.drawImage(image, x, y, width, height);
        
+       // #5
+       context?.restore();
      }
    }, [source, rotationAngle]);

    return (
      <>
+       <canvas ref={canvasRef} />
+       <button onClick={() => setRotationAngle((angle) => (angle + 90) % 360)}>회전</button>
      </>
    );
  }
```

회전하는 기능 하나만 추가했을 뿐인데 코드양이 상당히 늘어났습니다.

1. 전엔 canvas가 고정됐기 때문에 width와 height 또한 상수로 두고 사용할 수 있었지만, canvas가 회전하게 되면서 각도에 따라 width와 height이 서로 뒤바뀌기 때문에 따로 너비와 높이를 지정하는 로직을 추가했습니다.
1. 또한 앞서 설명했듯이 회전한 도형의 평행이동을 위해 `drawImage`의 left-top 시작점이 되는 `x`와 `y`를 회전 각도에 따라 계산합니다.
1. 그후에 회전한 이미지 크기대로 canvas 크기를 맞춥니다.
1. 회전에 필요한 모든 준비를 마쳤으니 `context.rotate`를 호출해 canvas를 회전시킵니다.
1. 회전이 끝나면 `context.restore`를 호출하여 canvas를 원래 위치로 되돌립니다.

여기서, `context.rotate` 후에 `context.restore()`를 호출하는 것을 잊지 않도록 주의해야 합니다. `context.restore()` 해주지 않는다면 이미 회전된 상태를 기준으로 다음 회전을 하기 때문입니다. 아래 이미지 처럼 말입니다.
![rotation without restore](/assets/images/2021-05-28-simple-image-editor/rotation-without-restore.jpeg)

반면에, `context.restore()`를 통해 image를 그릴 때 마다 canvas를 원래 위치로 되돌린다면 아래와 같은 순서로 이미지 회전이 이뤄집니다.

![rotation with restore](/assets/images/2021-05-28-simple-image-editor/rotation-with-restore.jpeg)


## Blur Image
블러 기능은 데모 gif에서 보았듯이 이미지 위에서 드래그를 통해 영역을 지정하면 그 영역에 블러를 적용하는 식으로 작동합니다.

드래그는 `mousemove` 이벤트를 통해 제어합니다.
`mousemove`의 이벤트 객체는 마우스 커서의 현재 위치 값인 `clientX`, `clientY`와 클릭 중인 마우스 버튼 정보인 `buttons` property를 제공하기 때문에 드래그 영역에 대한 값을 어려움 없이 계산할 수 있습니다.

다만, 드래그 도중엔 마우스의 좌표값이 수시로 변할텐데 그때마다 기존의 드래그 영역(사각형)을 지우고 새로운 영역을 그리는 작업이 필요합니다.
당연히 이렇게 사각형을 그리고 지우고 그리고 지우고 그리고... 하는 작업은 모두 re-rendering으로 처리할 것입니다.
그런데 앞서 canvas 위에 이미지를 띄워놓은 상태에서 이렇게 드래그 영역을 그리고 지우길 반복하는 작업은 다른 문제를 낳습니다.

`context.clearRect` 메소드로 특정 영역을 지우는 것은 할 수 있지만 canvas 상에 특정 도형만을 지우는 것은 <sub>(제가 아는 바로는)</sub> 불가능합니다.
즉, 드래그 영역을 표시하기 위해 사각형을 그리고, 다음 좌표값에 맞는 사각형을 그리기에 앞서 이전 사각형을 지울 때, 뒷 배경이 되는 이미지까지 전부 지워질 것이고, 새로운 사각형을 그리기 전에 뒷 배경 이미지를 다시 그린다고 해도, 사용자의 눈엔 re-rendering으로 인한 깜박거림이 나타날 수 밖에 없는 것입니다.
이게 별거 아니어보여도 드래그할 때마다 이미지가 계속 깜빡거려 눈을 굉장히 아프게 할 정도로 사용성을 상당히 저해하는 요소였습니다. 

flickering을 감수하고 사용한다고 하더라도, 쏟아지는 `mousemove` 이벤트 리스닝과 드래그 영역 + 다른 블러영역 + 전체 이미지 리렌더링을 웹브라우저 call stack이 감당하기 매우 버거운 작업이기 때문에 성능적으로도 만족스러운 결과를 내놓지 못합니다.

이를 해결하기 위해 이미지 편집 프로그램 등에서 layer를 사용하는 것에 착안하여, 여러 canvas를 선언한 뒤에 서로 다른 역할을 부여해 re-rendering의 범위를 분리하는 해결방안을 적용해 보았습니다.
layer는 아래와 같이 세 개의 canvas를 중첩한 형태로 구성했습니다.

![layers](/assets/images/2021-05-28-simple-image-editor/layers.jpeg)

layer 중첩은 별도의 css 설정으로 해결했으며 각 layer는 다음의 역할을 수행합니다.

* Layer2: 드래그 이벤트 리스닝, 드래그 영역 표시
* Layer1: 편집된 이미지 표시
* Layer0: 블러 처리된 전체 이미지 표시

layer2는 최상단에 위치하며 드래그 이벤트가 바인딩되어 있습니다. 그리고 드래그 이벤트가 발생할 때마다 드래그 영역을 표시해주는데, layer1보다 위에 있으며 투명하기 때문에 layer1의 이미지를 보여주며 동시에 layer2에서 생성하는 드래그 영역을 이미지 위에 표시할 수 있습니다.

layer1은 편집된 결과 이미지를 보여줍니다. 즉 블러 처리는 이 layer에 반영하며, 저장 또한 이 layer를 바탕으로 합니다.

layer0은 가장 아래에 위치한 layer이며 layer1과 2 아래에 있기에 사용자에게 보이진 않습니다. layer0에선 이미지 전체에 블러 처리가 된 이미지를 표시합니다. 드래그 시에 얻어지는 `x`, `y`, `width`, `height` 정보에 해당하는 영역을 `ImageData` 타입의 객체로 저장한 뒤에 layer1의 동일한 위치에 `imageData`를 노출시키는 것으로 블러 효과를 적용합니다.

정리하자면, 블러는 다음과 같은 순서로 처리됩니다.
1. 블러 효과를 적용한 전체 이미지 생성
2. 지정(드래그)한 구역의 블러 `ImageData`를 저장
3. 지정한 좌표(드래그 영역의 left-top)에 블러 `ImageData`를 붙임

### 1. 블러 효과를 적용한 전체 이미지 생성
블러 효과는 css에서 블러를 적용하는 것과 비슷하게 `context.filter = 'blur(radius)'` 와 같은 형태로 `context.filter`에 블러 값을 할당합니다.

```ts
import { useRef, useEffect, useState } from 'react'

interface Props {
  source: string
}

export default function ImageEditor({ source }: Props) {
  const blurLayer = useRef<HTMLCanvasElement>(null);
  const imageLayer = useRef<HTMLCanvasElement>(null);
  const dragLayer = useRef<HTMLCanvasElement>(null);

  const [rotationAngle, setRotationAngle] = useState<number>(0);
  
  // draw blurLayer
  useEffect(() => {
    const canvas = blurLayer.current;
    const context = canvas?.getContext('2d');

    const image = new Image();
    image.src = source;
    image.onload = () => {
      if(context) context.filter = 'blur(10px)';
      // ...
      context?.drawImage(image, x, y, width, height);   // 블러 효과 적용
    }
  }, [source, rotationAngle]);

  // ...
  // draw imageLayer
  // draw dragLayer
  // ...

  return (
    <canvas ref={blurLayer} />
    <canvas ref={imageLayer} />
    <canvas ref={dragLayer} />
  );
}
```

### 2. 지정(드래그)한 구역의 블러 `ImageData`를 저장
`ImageData`는 canvas 특정 영역을 픽셀 데이터로 표현하는 객체이며 `context.getImageData(left, top, width, height)` 메소드를 호출하여 리턴받습니다.

`context.getImageData()` 를 사용하여 layer0의 블러 처리된 전체 이미지에서 드래그한 영역에 해당하는 'ImageData'를 따로 떼어내는 것입니다.

또한 `context.getImageData()` 덕에 image의 회전방향을 고려하지 않아도 되며, layer0을 layer1과 정확히 겹쳐놨기 때문에 `x`, `y`, `width`, `height` 값을 따로 보정해주지 않아도 된다는 장점이 있습니다.

다만, `context.getImageData(left, top, width, height)`를 호출할 때 `width`와 `heigth` 값이 `0` 이면 error가 발생하기 때문에 이에 대한 에러 방어 로직이 필요합니다.

```ts
import { useRef, useEffect, useState } from 'react';
 
interface Area { 
  x: number
  y: number
  width: number
  height: number
}
 
interface Props { 
  source: string
}

export default function ImageEditor({ source }: Props) {
  const blurLayer = useRef<HTMLCanvasElement>(null);
  const imageLayer = useRef<HTMLCanvasElement>(null);
  const dragLayer = useRef<HTMLCanvasElement>(null);

  // 블러 영역의 ImageData
  const [blurryImage, setBlurryImage] = useState<ImageData>();

  // 드래그 영역의 x, y, width, height 값을 가지는 객체
  const [dragArea, setDragArea] = useState<Area>({ x: 0, y: 0, width: 0, height: 0 });

  // ...

  // 드래그가 끝나면 전체 블러 이미지 중 드래그 영역에 해당하는 ImageData를 추출하여 저장
  function handleMouseUp() {
    const canvas = blurLayer.current;
    const context = canvas?.getContext("2d");
    if (dragArea.width !== 0 && dragArea.height !== 0) {
      setBlurryImage(
        context?.getImageData(dragArea.x, dragArea.y, dragArea.width, dragArea.height)
      );
    }
  }

  return (
    <canvas ref={blurLayer} />
    <canvas ref={imageLayer} />
    <canvas ref={dragLayer} onMouseUp={handleMouseUp} />
  );
}
```


### 3. 지정한 좌표(드래그 영역의 left-top)에 블러 `ImageData`를 붙임
`context.getImageData()`로 얻어낸 imageData는 `context.putImageData(ImageData, dx, dy)` 메소드로 붙여넣습니다.

`context.getImageData()`에서 이미 이미지 크기까지 다 정해졌기 때문에 이미지를 넣을 위치인 `dx`, `dy`만을 구하면 layer1 상의 원하는 위치에 블러 이미지를 첨부하는 것이 가능합니다.

드래그를 우하향 방향으로 했다면 드래그 영역의 `width`와 `height` 값이 양수이기 때문에 별 문제가 되지 않지만, 왼쪽/위 방향은 `width` 혹은 `height` 값을 음수로 만들기 때문에 이를 계산해주는 로직이 필요합니다.

```ts
import { useRef, useEffect, useState } from 'react';

interface Area { 
  x: number
  y: number
  width: number
  height: number
}

interface Props {
  source: string
}

export default function ImageEditor({ source }: Props) {
  const blurLayer = useRef<HTMLCanvasElement>(null);
  const imageLayer = useRef<HTMLCanvasElement>(null);
  const dragLayer = useRef<HTMLCanvasElement>(null);
  
  const [blurryImage, setBlurryImage] = useState<ImageData>();
  const [dragArea, setDragArea] = useState<Area>({ x: 0, y: 0, width: 0, height: 0 });

  // ...

  // imageLayer에 블러 영역을 그림으로써 블러 효과 적용
  useEffect(() => {
    const canvas = imageLayer.current;
    const context = canvas?.getContext("2d");
    
    const image = new Image();
    image.src = source;
    image.onload = () => {
      // ...

      const left = dragArea.width > 0 ? dragArea.x : dragArea.x + dragArea.width;
      const top = dragArea.height > 0 ? dragArea.y : dragArea.y + dragArea.height;
      if(blurryImage) context?.putImageData(blurryImage, left, top);
    }
  }, [source, rotationAngle, blurryImage]);

  // ...

  return (
    <canvas ref={blurLayer} />
    <canvas ref={imageLayer} />
    <canvas ref={dragLayer} />
  );
}
```



## References
> [Canvas with React.js \| by Lucas Miranda \| Medium](https://medium.com/@pdx.lucasm/canvas-with-react-js-32e133c05258)
>
> [Using images - Web APIs \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Using_images#creating_an_image_from_scratch)
>
> [변형 (transformations) - Web API \| MDN](https://developer.mozilla.org/ko/docs/Web/API/Canvas_API/Tutorial/Transformations#rotating)
>
> [CanvasRenderingContext2D.filter - Web APIs \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/filter)
>
> [Pixel manipulation with canvas - Web APIs \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Pixel_manipulation_with_canvas)