# Using Amazon Simple Email Service with Django

장고에서 자동 메일을 발송할 때 전용 이메일 계정을 만들고 그 크레덴셜을 사용하여 이메일을 발송하는 방법을 써 왔는데, 정보가 바뀔 때마다 설정도 고쳐줘야 하고 발송 자체가 불안정한 경우가 많아 대안을 찾다가 아마존 SES라는 것을 발견했다.

### [Amazon SES]

AWS 서비스 중 하나로 대량 이메일 발송용 솔루션이다. 장점은 안정성, 빠른 전송 속도, 넉넉한 발송량 제한, 그리고 싼 가격.  
발송 1천 건당 $0.10이며, 심지어 EC2 인스턴스에서 구동 중인 어플리케이션을 통해 발송되는 메일이라면 **매월 62,000통까지 무료**다. 안 쓸 이유가 없다.

심지어 발신 뿐만 아니라 수신도 가능하다.


### 서비스 신청

1. 아마존 SES는 현재 아래의 3개 리전에서만 사용이 가능하다.
 - EU (Ireland)
 - US East (N. Virginia)
 - US West (Oregon)

2. AWS 콘솔의 `Identity Management` - `Email Addresses` 메뉴에서 메일 발송에 사용할 이메일 주소를 등록 후 인증해준다.

3. `Email Sending` - `Sending Statistics` 메뉴에 들어가서 `Request Increased Sending Limits` 버튼을 클릭해 다음처럼 사용 승인 요청을 한다. 승인에는 대략 3-4시간 정도 걸리는 것 같음.
```
Regarding: Service Limit Increase
Request - Limit: Desired Daily Sending Quota
Request - New limit value: 원하는 만큼
```
다른 항목은 눈치껏 적절히..

4. `SMTP Settings` 메뉴에서 `Create My SMTP Credentials` 버튼을 클릭해 IAM 유저를 생성한다.  
이 크레덴셜이 아마존 SES SMTP 서버의 로그인 정보가 된다.

5. 아마존에서 서비스 승인 메일이 날아오면 사용 준비 완료.


### 장고 연동

1. settings.py 파일에 다음과 같이 이메일 백엔드를 설정한다.  
```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'email-smtp.us-east-1.amazonaws.com'  # 선택 리전에 따라 다름
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = '{{ IAM 유저명 }}'
EMAIL_HOST_PASSWORD = '{{ IAM 패스워드 }}'
DEFAULT_FROM_EMAIL = '{{ 메일 발송에 사용할 이메일 주소 }}'
```

2. 끝났다. 하던 대로 [django.core.email] 모듈을 이용해 메일을 보내면 된다.  
```python
from django.core.mail import send_mail

send_mail(
    'Subject here',
    'Here is the message.',
    'from@example.com',
    ['to@example.com'],
    fail_silently=False,
)
```
참 쉽죠?


### 기타

EC2 어플리케이션에서 발송 시 월 6만 건 무료 / 어지간한 유명 솔루션도 전부 걸려있는 1회 시도당 발송량 제한이 없다는 것 이 두가지에서부터 이미 엄청난 메리트를 먹고 들어가며, 해당 이메일 주소의 명의를 빌려 AWS 자체 클라우드 서버를 통해 이메일을 발송하는 것이기 때문에 최초 이메일주소 인증만 되고 나면 해당 이메일의 크레덴셜은 더 이상 필요가 없게 된다. 극단적으로는 해당 이메일 주소를 없애도 발송은 원활히 동작한다.

다만 리전이 전부 바다 건너에 있는 물리적 한계 상, send rate는 표시된 14 emails/sec에는 많이 못 미치는 듯.  

그리고 수신자의 complaint rate가 높아질수록 daily quota와 send rate 모두 동적으로 감소한다고 한다. complaint란 수신자가 '스팸 메일로 신고' 또는 '스팸 편지함으로 보내기' 등의 액션을 취하면 발생하는 것으로서, ISP에서 이를 기록한 다음 ESP, 즉 아마존 SES에 전달한다고 한다. 이에 대한 자세한 설명은 [AWS 기술 블로그 글] 참고.


[Amazon SES]:https://aws.amazon.com/ses/
[django.core.email]:https://docs.djangoproject.com/en/2.0/topics/email/
[AWS 기술 블로그 글]:https://aws.amazon.com/blogs/ses/email-definitions-complaint-rate/
