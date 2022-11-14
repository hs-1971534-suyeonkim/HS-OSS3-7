# 오픈소스 11주차

##### :각자의 파일에 '좌석 예약'과 '음성 인식'에 관련된 오픈소스 및 자료 찾기

<br>
<hr>

## 서비스 구상도 

<img width="1091" alt="스크린샷 2022-11-14 오후 3 59 30" src="https://user-images.githubusercontent.com/117635203/201595917-286997b0-1fdb-4d02-8238-0a2cbaf205c7.png">


- 기존 출석체크앱의 업데이트 버전이다.
- 초기 사용시 학생이 학생증을 등록해야한다.
- 학생증 인증시 학교 재학생 정보 DB정보와 일치하면 인증된다. (+추가 기능 : 학생증 QR(or 바코드) 에서 학생정보 받아오는 오픈소스 추가해도 좋을거같음)
- 인증될 시 현 학기 수강신청한 수업의 정보를 불러온다.
- 수업 정보 -> 학교 전체 수업 시간과 그에 맞는 강의실 정보
- 학생은 실시간 대면수업 15분 전 수업 좌석을 선택할 수 있다.
- 수업인증 방식은 두가지 : 음성인식 , NFC tag
- 좌석 선택 -> 강의실 배치도 
- 강의실 좌석에는 NFC tag 스티커가 다 부착되어있다.
- 선택한 좌석의 핸드폰의 NFC를 맞대면 출석인증된다.
- 음성인식을 통한 출석체크


## 선택한 오픈소스
##### : NFC Tag를 이용한 출석체크 인증

<br>
1. 선정이유
<br>
NFC(Near Field Communication) 근거리 무선 통신으로 비접촉 통신기술이다. 
일단 약 40m까지 통신하는 블루투스와 다르게 NFC는 10cm이내의 아주 가까운 거리의 무선통신을 하며 다른 기술들보다 보안적인 문제와 비용적인 문제에서 합리적이기 때문에 NFC기술을 통한 출석체크 인증 방식을 생각했다.기존의 블루투스를 활용한 출석체크의 경우 강의실에 직접들어가지 않아도 출석체크가 되는 상황에 학생들이 수업을 듣지앟아도 출석체크가 될 수 있었다. 그리고 현재 거의 모든 폰에 NFC가 들어가있고 현재 출입통제나 삼성페이등등 여러 서비스에서 사용되고 있어서 
<br>
2. 적용?<br>
핸드폰 NFC와 선택한 좌석의 NFC tag 스티커가 맞닿으면 학교시스템에 정보(기기 이름, 선택한 좌석인지?, 수업 맞는지..?)가 넘어가서  출석인증이 되게한다.
<br>
3. 오프소스 링크 및 코드<br>
참고할거-안드로이드 출입인증 NFC
[ex1](http://isweb.joongbu.ac.kr/~jbuis/2014/report-2014-3.pdf
)
<br>
참조할거-NFC Tag를 이용한 출입통제 시스템
[ex2](https://scienceon.kisti.re.kr/commons/util/originalView.do?cn=CFKO201435553769306&oCn=NPAP12684394&dbt=CFKO&journal=NPRO00377534)

<pre>
>>NFC Tagging<<
int msgSize = nfc.read(ndefBuf, sizeof(ndefBuf));
if (msgSize > 0) {
NdefMessage msg = NdefMessage(ndefBuf, msgSize);
int recordCount = msg.getRecordCount();
for (int i 0;i<recordCount;i++)
{
NdefRecord record = msg.getRecord(i);
int payloadLength = record.getPayloadLength();
byte payload[payloadLength];
record.getPayload(payload);
String payloadAsString = "";
}
}
</pre>
##USER인증
학교DB에서 재학생 data 뽑아내기-> 학생증 인증 (+ 사진 QR코드에서 정보뽑아내기 오픈소스)
<br>

####QR코드, 바코드 리더 라이브러리 Zxing <br>
######https://namu.wiki/w/ZXing
구글에서 제공하는 오픈소스로 Zebra Crossing의 약자 <br>
다양한 바코드도 인식할 수 있음 -> 약 15가지정도
<기능>
- 카메라를 연 후 프리뷰를 가동한다. 
- 카메라로부터 지속적으로 영상을 받아들인다. [2]
- 영상에서 밝기값만 추출[3]하여 이를 기반으로 이진화를 수행한다.[4]
- Detector 클래스를 통해 QR코드 영역을 찾아냈다. 
- 찾아낸 영역을 Decoder 클래스를 통해 해석한다. 
- 결과 값과 결과 영상을 리턴
- 결과 값을 분석하여 URL일 경우 탭하면 인터넷으로 연결되도록 한다. 
- 화면에 결과 영상과 결과 값을 출력한다.
<사용법 - 안드로이드>
- Gradle 추가
<pre>
apply plugin: 'com.android.application'
 
android {
    compileSdkVersion 25
    buildToolsVersion "26.0.0"
    ...
}
 
dependencies {
    ...
<span style="color: rgb(255, 0, 0);">    compile 'com.journeyapps:zxing-android-embedded:3.2.0@aar'
    compile 'com.google.zxing:core:3.2.1'</span>
    ...
}
</pre>
<br>
![Zxing](https://namjackson.tistory.com/15)
<img width="807" alt="스크린샷 2022-11-14 오후 5 15 12" src="https://user-images.githubusercontent.com/117635203/201608913-dc4684be-36e5-45f8-8621-56dce981f2bb.png">


##좌석예약
수업 15분 전 수업좌석을 예약할 수 있다. 15분 전
<br>

학교 DB에서 가져오는 법
<br>
https://velog.io/@junseok816/%ED%95%99%EA%B5%90-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EA%B2%B0%EA%B3%BC%EB%AC%BC%EB%A1%9C-%EC%82%AC%EC%9D%B4%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B02-%EC%82%AC%EC%9D%B4%ED%8A%B8%EC%97%90-DB%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0
<br>좌석예약 오픈소스<br>
https://github.com/boostcamp-2020/Project07-A-RealTime-Seat-Reservation-System
<br>DB 데이터 가져오기<br>
https://dk-projects.tistory.com/11

<hr>
