# COVI SDK 리액트 연동 가이드
<details>
<summary>COVI SDK 기능만 사용할 때 <strong>리액트 라이브러리</strong>와 연동하는 방법을 안내합니다.</summary>
<div>
<ul>
<li>COVI SDK가 제공하는 기능 : 이벤트 트래킹 로그 전송, 가시성 체크, VAST 3.0 XML 요청 및 응답결과 파싱</li>  
</ul>
</div>
</details>

<details>
<summary>VAST 3.0을 지원하지 않는 HTML5 미디어 플레이어를 대상으로 지원합니다.</summary>
<div>
<ul>
<li>className이 'covi_player'인 video 요소에 COVI SDK를 적용함으로써 HTML5 미디어 플레이어가 VAST 3.0을 지원할 수 있도록 합니다.</li>  
</ul>
</div>
</details>

<br>

### 목차
  + [React 함수형 컴포넌트 연동](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#react-%ED%95%A8%EC%88%98%ED%98%95-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EC%97%B0%EB%8F%99)

    - [Script 태그 동적 삽입](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#script-%ED%83%9C%EA%B7%B8-%EB%8F%99%EC%A0%81-%EC%82%BD%EC%9E%85)
      * [`useEffect`로 Script 태그 삽입 로직 처리](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#1-useeffect%EB%A1%9C-script-%ED%83%9C%EA%B7%B8-%EC%82%BD%EC%9E%85-%EB%A1%9C%EC%A7%81%EC%9D%84-%EC%B2%98%EB%A6%AC%ED%95%A9%EB%8B%88%EB%8B%A4)
      * [`src/customHooks` 디렉터리에서 `useScript.jsx` 파일 생성](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#2-srccustomhooks-%EB%94%94%EB%A0%89%ED%84%B0%EB%A6%AC%EC%97%90%EC%84%9C-usescriptjsx-%ED%8C%8C%EC%9D%BC%EC%9D%84-%EC%83%9D%EC%84%B1%ED%95%A9%EB%8B%88%EB%8B%A4)

    - [COVI SDK 적용하기](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#covi-sdk-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0)
      * [`PlayerSample` 컴포넌트 생성](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#1-classname%EC%9D%B4-covi_player%EC%9D%B8-video-element%EB%A5%BC-%EB%A6%AC%ED%84%B4%ED%95%98%EB%8A%94-playersample-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%A5%BC-%EC%83%9D%EC%84%B1%ED%95%A9%EB%8B%88%EB%8B%A4)
      * [SDK CDN 정보](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#2-sdk-%EC%A0%95%EB%B3%B4)
      * [`PlayerSample` 컴포넌트에 COVI SDK 적용](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#3-%ED%8E%98%EC%9D%B4%EC%A7%80%EB%A5%BC-%EB%A0%8C%EB%8D%94%EB%A7%81%ED%95%98%EB%8A%94-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%97%90%EC%84%9C-usescript%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4-%EC%A0%95%EC%83%81%EC%A0%81%EC%9C%BC%EB%A1%9C-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EB%A1%9C%EB%93%9C%ED%96%88%EC%9D%84-%EB%95%8C-playersample-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%97%90-covi-sdk%EB%A5%BC-%EC%A0%81%EC%9A%A9%ED%95%A9%EB%8B%88%EB%8B%A4)

    - [SDK 내장 함수 사용](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#sdk-%EB%82%B4%EC%9E%A5-%ED%95%A8%EC%88%98-%EC%82%AC%EC%9A%A9)
      * [runCoviSdk()](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#1-runcovisdk)
      * [initCoviSdk()](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#2-initcovisdk)
      * [coviClickLandingButton()](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#3-coviclicklandingbutton)

  + [이벤트 트래킹 로그 전송 체크](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%8A%B8%EB%9E%98%ED%82%B9-%EB%A1%9C%EA%B7%B8-%EC%A0%84%EC%86%A1-%EC%B2%B4%ED%81%AC)
    - [이벤트 트래킹(Event Tracking)](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#1-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%8A%B8%EB%9E%98%ED%82%B9event-tracking)
    - [COVI SDK가 적용된 비디오 플레이어의 진행 시간에 따른 이벤트 트래킹 체크](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#2-1-covi-sdk%EA%B0%80-%EC%A0%81%EC%9A%A9%EB%90%9C-%EB%B9%84%EB%94%94%EC%98%A4-%ED%94%8C%EB%A0%88%EC%9D%B4%EC%96%B4%EC%9D%98-%EC%A7%84%ED%96%89-%EC%8B%9C%EA%B0%84%EC%97%90-%EB%94%B0%EB%A5%B8-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%8A%B8%EB%9E%98%ED%82%B9-%EC%B2%B4%ED%81%AC)
    - [클릭을 통해 광고주 랜딩 페이지로 이동했을 때 이벤트 트래킹 체크](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#2-2-%ED%81%B4%EB%A6%AD%EC%9D%84-%ED%86%B5%ED%95%B4-%EA%B4%91%EA%B3%A0%EC%A3%BC-%EB%9E%9C%EB%94%A9-%ED%8E%98%EC%9D%B4%EC%A7%80%EB%A1%9C-%EC%9D%B4%EB%8F%99%ED%96%88%EC%9D%84-%EB%95%8C-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%8A%B8%EB%9E%98%ED%82%B9-%EC%B2%B4%ED%81%AC)
    - [이벤트 트래킹 로그별 전송 시점](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#3-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%8A%B8%EB%9E%98%ED%82%B9-%EB%A1%9C%EA%B7%B8%EB%B3%84-%EC%A0%84%EC%86%A1-%EC%8B%9C%EC%A0%90)

  + [가시성 측정 체크](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#%EA%B0%80%EC%8B%9C%EC%84%B1-%EC%B8%A1%EC%A0%95-%EC%B2%B4%ED%81%AC)
    - [가시성(Viewability)](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#1-%EA%B0%80%EC%8B%9C%EC%84%B1viewability)
    - [가시성 측정 프로세스](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#2-%EA%B0%80%EC%8B%9C%EC%84%B1-%EC%B8%A1%EC%A0%95-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4)
    - [COVI SDK가 적용된 동영상 플레이어의 가시성 측정이 정상 작동하는지 체크](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#3-covi-sdk%EA%B0%80-%EC%A0%81%EC%9A%A9%EB%90%9C-%EB%8F%99%EC%98%81%EC%83%81-%ED%94%8C%EB%A0%88%EC%9D%B4%EC%96%B4%EC%9D%98-%EA%B0%80%EC%8B%9C%EC%84%B1-%EC%B8%A1%EC%A0%95%EC%9D%B4-%EC%A0%95%EC%83%81-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EC%A7%80-%EC%B2%B4%ED%81%AC%ED%95%98%EA%B8%B0)

  + [참조](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#%EC%B0%B8%EC%A1%B0)

<br>

## React 함수형 컴포넌트 연동

* 함수형 컴포넌트로 작성된 환경에서 매체사에서 생성한 비디오 플레이어(video element)에 **HTML5 COVI SDK를 적용하는 예제**입니다. <p>

* 아래 React 함수형 컴포넌트 연동 예제를 실행하기 위해 `react >= 18.2.0`, `react-dom >= 18.2.0` 설치를 권장합니다.

<br>

## Script 태그 동적 삽입

### 1) `useEffect`로 Script 태그 삽입 로직을 처리합니다.
  - [`useEffect`](https://react.dev/reference/react/useEffect) : 함수형 컴포넌트에서 외부 시스템과의 동기화, 비동기화를 제어할 때 사용하는 리액트 hooks

<br>

### 2) `src/customHooks` 디렉터리에서 [`useScript.jsx`](https://usehooks.com/usescript) 파일을 생성합니다.
  - url 파라미터로 받아온 주소로 스크립트를 생성하고, 생성한 스크립트의 상태를 리턴합니다.

```
import { useState, useEffect } from 'react';

export default function useScript(url) {
    const [status, setStatus] = useState(url ? 'loading' : 'error');

    useEffect(() => {
        if (!url) {
            setStatus('error');
            return;
        }

        const setStateFromEvent = (event) => {
            setStatus(event.type === 'load' ? 'ready' : 'error');
        };

        let script = document.querySelector(`script[src="${url}"]`);

        if (!script) {
            script = document.createElement('script');
            script.src = url;
            script.async = true;
            script.setAttribute('data-status', 'loading');
            document.head.appendChild(script);

            const setAttributeFromEvent = (event) => {
                script.setAttribute('data-status', event.type === 'load' ? 'ready' : 'error');
                setStateFromEvent(event.type);
            };

            script.addEventListener('load', setAttributeFromEvent);
            script.addEventListener('error', setAttributeFromEvent);
        } else {
            setStatus(script.getAttribute('data-status'));
        }

        script.addEventListener('load', setStateFromEvent);
        script.addEventListener('error', setStateFromEvent);

        return () => {
            if (script) {
                script.removeEventListener('load', setStateFromEvent);
                script.removeEventListener('error', setStateFromEvent);
            }
        };
    }, [url]);

    return status;
}
```

<br>

## COVI SDK 적용하기


### 1) `className`이 `covi_player`인 video element를 리턴하는 `PlayerSample` 컴포넌트를 생성합니다.
  - 동영상 플레이어를 인라인으로 재생하기 위해 video element에 [`playsInline`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video#playsinline) 속성을 추가합니다.
  - 동영상 플레이어를 자동재생하기 위해 video element에 `autoPlay`, `muted` 속성을 추가합니다.

```
export const PlayerSample = () => {
    return <video className='covi_player' playsInline autoPlay muted></video>;
};
```

<br>

### 2) SDK 정보
- Covi SDK URL
```
https://covi-plat-file.beta.covi.co.kr/player/js/coviplayer.js
```

- Publisher 스크립트 URL
```
매체 제휴를 진행하면서 전달받은 Publisher 스크립트 URL 사용 (전달받은 Publisher 스크립트 URL이 없을 시 COVI 개발자에게 문의해주세요)
```

> 위 SDK 스크립트는 개발용입니다. 상용 서비스 전환 시 SDK 스크립트의 URL이 달라지므로 상용 URL을 받아서 사용해야 합니다.
>
> 개발용 SDK를 적용해서 연동을 마친 후 COVI 매체 제휴 담당자에게 상용 URL 주소를 문의해 주세요.

<br>

### 3) 페이지를 렌더링하는 컴포넌트에서 `useScript`를 사용해 정상적으로 스크립트를 로드했을 때 `PlayerSample` 컴포넌트에 COVI SDK를 적용합니다.
- `coviScriptStatus`와 `publisherScriptStatus`가 준비됐을 때 COVI SDK를 로드합니다.
- `runCoviSdk`와 `initCoviSdk`은 [COVI SDK 내장 함수를 사용](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#sdk-%EB%82%B4%EC%9E%A5-%ED%95%A8%EC%88%98-%EC%82%AC%EC%9A%A9)합니다.

```
import { useEffect } from 'react';
import useScript from '../customHooks/useSrcipt';
import { PlayerSample } from '../components/PlayerSample';

const FirstPage = () => {

    const [isCoviSdkFirstLoaded, setIsCoviSdkFirstLoaded] = useState(true);

    // useScript의 인수(coviscript src, publisher src)값은 COVI 개발자에게 문의해주세요.
    const [coviScriptStatus, publisherScriptStatus] = [useScript('coviscript src'), useScript('publisher src')];

    useEffect(() => {
        if (coviScriptStatus === 'ready' && publisherScriptStatus === 'ready' && isCoviSdkFirstLoaded === true) {
            if (typeof runCoviSdk === 'function') {
                runCoviSdk();
                setIsCoviSdkFirstLoaded((isCoviSdkFirstLoaded) => false);
            }
        } else if (coviScriptStatus === 'ready' && publisherScriptStatus === 'ready' && isCoviSdkFirstLoaded === false) {
            if (typeof initCoviSdk === 'function') initCoviSdk();
        }
    }, [coviScriptStatus, publisherScriptStatus]);

    return (
        <>
          <PlayerSample />
        </>
    )
}
```

## SDK 내장 함수 사용
  - 전역 window 객체에서 runCoviSdk, initCoviSdk, coviClickLandingButton 함수를 추출합니다.
```
const { runCoviSdk, initCoviSdk, coviClickLandingButton } = window
```

### 1) runCoviSdk()
최초 COVI SDK를 로드할 때 사용합니다.

### 2) initCoviSdk()
기존 COVI SDK 및 트래킹 로그들을 초기화하고 새로운 광고를 호출 합니다.

### 3) coviClickLandingButton()
광고주의 랜딩 페이지를 새 탭으로 엽니다.

<br>

## 이벤트 트래킹 로그 전송 체크

### 1) 이벤트 트래킹(Event Tracking)
광고 캠페인의 성과를 측정하기 위해 사용되며, 상황에 따른 유저의 행동을 추적하는 트래킹 로그를 전송합니다.

<br>

### 2-1) COVI SDK가 적용된 비디오 플레이어의 진행 시간에 따른 이벤트 트래킹 체크
![image](https://github.com/covigroup/covi-html5-sdk-react/assets/122589688/0993424c-afcf-4f3e-8818-1a524dca5c84)

- ① 크롬 브라우저에서 개발자 도구(F12 / opt+cmd+i)를 엽니다.

- ② 네트워크 탭으로 이동합니다.

- ③ 필터창에 ‘covi’를 입력합니다.

- ④ Fetch/XHR 탭을 클릭합니다.

- ⑤ `imp`, `atp`, `sec2`, `vimp`, `qtr1`, `qtr2`, `qtr3`, `qtr4`, `sec15`, `sec30` 트래킹 로그가 정상적으로 전송됐는지(status:200) 확인합니다.

<br>

### 2-2) 클릭을 통해 광고주 랜딩 페이지로 이동했을 때 이벤트 트래킹 체크
- `click` 트래킹 로그가 정상적으로 전송되는지(status:200) 확인합니다.

![image](https://github.com/covigroup/covi-html5-sdk-react/assets/122589688/a4378465-234e-457a-a22c-87f0cf6d7fa2)

<br>

### 3) 이벤트 트래킹 로그별 전송 시점
- **`imp`** : 비디오 플레이어가 화면에 100% 노출 시 `imp` 로그를 전송합니다.
- **`start(atp or ctp)`** : 광고 재생 시작 시 전송되며 autoplay일 경우 `atp`, clicktopaly일 경우에는 `ctp` 로그를 전송합니다.
- **`sec2`, `vimp`** : 광고 영상이 2초 재생됐을 때 `sec2`, 3초 재생됐을 때 `vimp` 로그를 전송합니다.
- **`qtr1`, `qtr2`, `qtr3`, `qtr4`** : 광고 영상 길이(duration)를 기준으로 1/4, 2/4, 3/4, 4/4 재생 됐을 때 각 로그들을 전송합니다.
- **`sec15`, `sec30`** : 광고 영상이 15초 재생됐을 때 `sec15`, 30초 재생됐을 때 `sec30` 로그를 전송합니다. 
  + ex) 15초 길이의 광고 영상은 `sec15` 로그까지만 전송하고, 30초 길이의 광고 영상은 `sec15, sec30` 로그를 모두 전송합니다.
- **`click`** : [`coviClickLandingButton`](https://github.com/covigroup/covi-html5-sdk-react?tab=readme-ov-file#3-coviclicklandingbutton)이 onClick으로 등록된 요소를 클릭했을 때 `click` 로그를 전송합니다. 

<br>

## 가시성 측정 체크

### 1) 가시성(Viewability)
유저에게 게재된 광고가 보이는지 유무를 바탕으로 하는 지표로 COVI SDK가 적용된 동영상 플레이어가 가시적(viewable)인지를 측정합니다.

### 2) 가시성 측정 프로세스

![image](https://github.com/covigroup/covi-html5-sdk-react/assets/122589688/976df076-7a8b-4014-bf2d-0475d505b104)

### 3) COVI SDK가 적용된 동영상 플레이어의 가시성 측정이 정상 작동하는지 체크하기

- [x] 화면에 동영상 플레이어가 100% 노출 될 때 재생 되는지 확인
- [x] 화면에 동영상 플레이어기 100% 노출 안될 때 일시 정지 되는지 확인

<br>

## 참조
|Topic|URL|
|---|:---:|
|covi-html5-sdk 개발 가이드|https://github.com/covigroup/covi-html5-sdk/wiki|
|React 함수형 컴포넌트 공식문서|https://react.dev/blog/2023/03/16/introducing-react-dev|
|useEffect Hooks|https://react.dev/reference/react/useEffect|
|usehooks 라이브러리 / useScript|https://usehooks.com/usescript|
|playsInline / mdn|https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video#playsinline|
