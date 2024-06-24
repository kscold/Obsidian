- 자바스크립트에서 Blob(Binary Large Object, 블랍)은 이미지, 사운드, 비디오와 같은 멀티미디어 데이터를 다룰 때 사용할 수 있다.

- 대개 데이터의 크기(Byte) 및 MIME 타입을 알아내거나, 데이터를 송수신을 위한 작은 Blob 객체로 나누는 등의 작업에 사용한다.

- File 객체도 `name`과 `lastModifiedDate` 속성이 존재하는 Blob 객체이다.

## 문법

- Blob 생성자는 새로운 Blob 객체를 반환한다.
- 생성 시 인수로 `array`와 `options`을 받는다.

```js
const newBlob = new Blob(array, options);
```

### [[배열(Array)]]

- Blob 생성자의 첫번째 인자로 [ArrayBuffer](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer), [ArrayBufferView](https://developer.mozilla.org/en-US/docs/Web/API/ArrayBufferView), Blob([File](https://developer.mozilla.org/ko/docs/Web/API/File)), [DOMString](https://developer.mozilla.org/ko/docs/Web/API/DOMString) 객체 또는 이러한 객체가 혼합된 Array를 사용할 수 있다.

```js
new Blob([new ArrayBuffer(data)], { type: 'video/mp4' });
new Blob(new Uint8Array(data), { type: 'image/png' });  
new Blob(['<div>Hello Blob!</div>'], {
	type: 'text/html',
	endings: 'native'
});
```

### options

옵션으로는 `type`과 `endings`를 설정할 수 있습니다.  
`type`은 데이터의 [MIME 타입](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Complete_list_of_MIME_types)을 설정하며, 기본값은 `""` 입니다.  
`endings`는 `\n`을 포함하는 문자열 처리를 `"transparent"`와 `"native"`로 지정할 수 있으며, 기본값은 `"transparent"`입니다.

# Blob 객체[](https://heropy.blog/2019/02/28/blob/#e7640397-5d72-475d-8648-061036322112)

## Properties[](https://heropy.blog/2019/02/28/blob/#81dabed1-cb21-447a-a384-383cdf56d3d8)

다음은 약 43KB의 PNG 이미지로 생성한 Blob 객체 입니다.

![Blob Object](https://heropy.blog/images/screenshot/blob/blob-object.jpg)

생성자를 통해 만들어진 Blob 객체는 `size`, `type`의 속성을 가집니다.  
`size`는 Blob 객체의 바이트(Byte) 단위 크기를 의미하며, `type`은 객체의 MIME 타입을 의미합니다.  
MIME 타입을 알 수 없는 경우 빈 문자열(`""`)이 할당됩니다.

## [[메서드(Method)]]

- Blob 객체에서 사용할 수 있는 slice() [[메서드(Method)]]는 지정된 바이트 범위의 데이터를 포함하는 새로운 Blob 객체를 만드는 데 사용된다.
- 10MB 이상 사이즈가 큰 Blob 객체를 작게 조각내어 사용할 때 유용합니다.

```js
const blob = new Blob();  // New blob object
blob.slice(start, end, type);
```

- `start`는 시작 범위(Byte, Number), `end`는 종료 범위(Byte, Number), `type`은 새로운 Blob 객체의 MIME 타입(String)을 지정한다.

```js
// Blob 객체(blob)에서 첫 1KB의 JPG Blob 객체(chunk)를 생성
const chunk = blob.slice(0, 1024, 'image/jpeg');
```

다음 예제는 위에서 살펴본 Blob 객체(약 43KB의 PNG 이미지로 생성한)를 10개의 Chunk로 조각냅니다.  
그리고 각 Chunk를 `chunks` 배열에 순서대로 저장합니다.

```
const chunks = [];
const numberOfSlices = 10;
const chunkSize = Math.ceil(blob.size / numberOfSlices);
for (let i = 0; i < numberOfSlices; i += 1) {
  const startByte = chunkSize * i;
  chunks.push(
    blob.slice(
      startByte,
      startByte + chunkSize,
      blob.type
    )
  );
}
console.log(chunks);  // This is as follows..
```

![Blob Slice](https://heropy.blog/images/screenshot/blob/blob-object-slice.jpg)

이렇게 조각낸 Blob 객체들(Chunks)은 필요에 의해 간단하게 다시 합칠 수 있습니다.

```js
const mergedBlob = new Blob(
  chunks,
  { type: blob.type }
);
document.getElementById('merged-image').src = window.URL.createObjectURL(mergedBlob);
document.getElementById('chunk-image').src = window.URL.createObjectURL(chunk[0]);

// Revoke Blob URL..
```

아래 이미지는 위 코드의 결과로 왼쪽 이미지는 `#merged-image` 요소, 오른쪽 이미지는 `#chunk-image` 요소입니다.  
오른쪽 이미지가 약 1/10로 잘려서 출력되는 것을 볼 수 있습니다.

> 이미지를 시각적으로 분리(조각)하는 용도는 아니며, 이해를 돕기 위해서 첨부합니다.

![Blob Original image VS Sliced image](https://heropy.blog/images/screenshot/blob/blob-origin-vs-sliced.jpg)

위 코드의 마지막을 보면 `URL.createObjectURL()`을 사용하였으며, 이는 Blob 객체를 가리키는 URL을 생성하여 [[DOM(Document Object Model)]]에서 참조할 수 있게 합니다.  
[[Blob URL]]에 대해서 간단하게 알아봅시다.