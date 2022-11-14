# 오픈소스 11주차

##### :각자의 파일에 '좌석 예약'과 '음성 인식'에 관련된 오픈소스 및 자료 찾기

<br>
<hr>

## 서비스 구상도 

<img width="816" alt="스크린샷 2022-11-14 오전 11 05 24" src="https://user-images.githubusercontent.com/117635203/201564912-4365edd0-bb08-41c7-a708-044b2abd95a1.png">

- 기존 출석체크앱의 업데이트 버전이다.
- 초기 사용시 학생이 학생증을 등록해야한다.
- 학생증 인증시 학교 재학생 정보 DB정보와 일치하면 인증된다. 
- 인증될 시 현 학기 수강신청한 수업의 정보를 불러온다.
- 수업 정보 -> 학교 전체 수업 시간과 그에 맞는 강의실 정보
- 학생은 실시간 대면수업 15분 전 수업 좌석을 선택할 수 있다.
- 좌석 선택 -> 강의실 배치도 
- 강의실 좌석에는 NFC tag 스티커가 다 부착되어있다.
- 선택한 좌석의 핸드폰의 NFC를 맞대면 출석인증된다.


## 선택한 오픈소스
##### : NFC Tag를 이용한 출석체크 인증

<br>
1. 선정이유<br>
NFC(Near Field Communication) 근거리 무선 통신으로 비접촉 통신기술이다. 
일단 약 40m까지 통신하는 블루투스와 다르게 NFC는 10cm이내의 아주 가까운 거리의 무선통신을 하며 다른 기술들보다 보안적인 문제와 비용적인 문제에서 합리적이기 때문에 NFC기술을 통한 출석체크 인증 방식을 생각했다.현재 거의 모든 폰에 NFC가 들어가있고 현재 출입통제나 삼성페이등등 여러 서비스에서 사용되고 있어서 사용하는 것에 문제가 없을 것이다.
<br>
2. 적용?<br>
핸드폰 NFC와 선택한 좌석의 NFC tag 스티커가 맞닿으면 학교시스템에 정보(기기 이름, 선택한 좌석인지?, 수업 맞는지..?)가 넘어가서  출석인증이 되게한다.
<br>
3. 오프소스 링크 및 코드<br>
참고할거-안드로이드 출입인증 NFC
[ex1](http://isweb.joongbu.ac.kr/~jbuis/2014/report-2014-3.pdf
)
참조할거-NFC Tag를 이용한 출입통제 시스템
[ex2](https://scienceon.kisti.re.kr/commons/util/originalView.do?cn=CFKO201435553769306&oCn=NPAP12684394&dbt=CFKO&journal=NPRO00377534)
```
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
```

