# Google Calendar Hack

### 월 말일마다 반복되는 이벤트 만들기

1. 아래와 같은 내용으로 임의의 `*.ics` 파일을 생성하고, `DTSTART` 뒤의 값을 반복 이벤트를 시작할 첫번째 월말일로 수정해준다. (예: 20190630)  
`SUMMARY` 및 `DESCRIPTION`도 적절히 수정하면 좋지만, 어차피 나중에 다시 수정할 수 있으므로 크게 고민할 필요는 없다.

```
BEGIN:VCALENDAR
VERSION:2.0
BEGIN:VEVENT
RRULE:FREQ=MONTHLY;INTERVAL=1;BYSETPOS=-1;BYDAY=SU,MO,TU,WE,TH,FR,SA
SUMMARY:{{이벤트명}}
DTSTART;VALUE=DATE:{{시작일}}
SEQUENCE:0
DESCRIPTION:{{이벤트 설명}}
END:VEVENT
END:VCALENDAR
```

2. 구글 캘린더 좌측 하단의 `Other calendars` 섹션에 있는 + 버튼을 누른 다음 `Import` 메뉴를 선택한다.

3. 방금 만들었던 `*.ics` 파일을 불러온 다음, 이벤트를 생성할 캘린더를 지정해준다.

4. 자 이제 월말일마다 반복되는 이벤트가 생성되었다.  

이렇게 생성된 이벤트는 추가 과정만 복잡했을 뿐 일반적인 캘린더 이벤트와 똑같다. 수정 버튼을 눌러서 이벤트 요약, 설명, 시간, 알람 등의 설정을 고칠 수도 있고, 수정 범위를 `All events`로 선택하면 그 이후의 모든 반복 이벤트에도 동일하게 적용된다.

즉 구글은 반복일 지정에 -1 인덱싱 옵션 하나만 추가하면 되는 간단한 수고를 여태껏 하지 않아서 사람을 번거롭게 만들고 있는 것이다. ~~이런 쓰레기들아~~
