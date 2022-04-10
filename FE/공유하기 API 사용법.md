# 공유하기 API 사용법

### 페이스북

참고문서: [https://developers.facebook.com/docs/sharing/reference/share-dialog](https://developers.facebook.com/docs/sharing/reference/share-dialog)
공유하기 링크 포맷
`https://www.facebook.com/dialog/share?display=popup&app_id=` `````페이스북 APP ID````` `&href=` `````encodeURIComponent(공유할 URL)````` `&redirect_uri=` ````encodeURIComponent(공유 후, 리다이렉트할 URL)````

`redirect_uri` 쿼리는 필수 값 아니므로 생략 가능

<br>

### 라인

참고문서: [https://social-plugins.line.me/ko/how\_to\_install#lineitbutton](https://social-plugins.line.me/ko/how_to_install#lineitbutton)
공유하기 링크 포맷
`https://social-plugins.line.me/lineit/share?url=` `````encodeURIComponent(공유할 URL)`````

<br>

### 카카오톡

참고문서: [https://developers.kakao.com/sdk/reference/js/release/Kakao.Link.html#DefaultFeedSettings](https://developers.kakao.com/sdk/reference/js/release/Kakao.Link.html#DefaultFeedSettings)

1. 카카오링크API는 javascript SDK 필요 (참고: [https://developers.kakao.com/docs/latest/ko/sdk-download/js](https://developers.kakao.com/docs/latest/ko/sdk-download/js)
    * `<script src="https://developers.kakao.com/sdk/js/kakao.min.js">` 구문을 body 태그 맨 아랫부분에 삽입하거나,
    * 모던 javascript 에서는 javascript sdk 파일을 다운받은 후, `require('./your_path/kakao.min');` 구문을 이용하여 적용가능
2. 다양한 형태의 메세지 템플릿 지원 (참고: [https://developers.kakao.com/docs/latest/ko/message/message-template](https://developers.kakao.com/docs/latest/ko/message/message-template)

기본 공유용 메세지 예시

``` javascript
Kakao.Link.sendDefault({
  objectType: 'feed',
  content: {
    title: '디저트 사진',
    description: '아메리카노, 빵, 케익',
    imageUrl:
      'http://mud-kage.kakao.co.kr/dn/NTmhS/btqfEUdFAUf/FjKzkZsnoeE4o19klTOVI1/openlink_640x640s.jpg',
    link: {
      mobileWebUrl: 'https://developers.kakao.com',
      androidExecParams: 'test',
    },
  },
  social: {
    likeCount: 10,
    commentCount: 20,
    sharedCount: 30,
  },
  buttons: [
    {
      title: '웹으로 이동',
      link: {
        mobileWebUrl: 'https://developers.kakao.com',
      },
    },
    {
      title: '앱으로 이동',
      link: {
        mobileWebUrl: 'https://developers.kakao.com',
      },
    },
  ]
});
```

<br>

### SMS

공유하기 링크 포맷

#### iOS

`sms:&body=` `````encodeURIComponent(SMS로 공유할 내용)`````

<br>

#### AOS

`sms:?body=` `````encodeURIComponent(SMS로 공유할 내용)`````