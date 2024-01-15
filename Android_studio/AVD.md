Android Emulator hyperdriver is not installed

```
Android SDK is up to date.
Running Android Emulator hypervisor driver installer
[SC] ControlService 실패 1062:

서비스가 시작되지 않았습니다.

[SC] DeleteService 성공
[SC] 4294967201 오류가 발생하여 StartService이(가) 실패했습니다.
Done

```

AMD CPU에서 가상화 관련 에러.
작업관리자 - 성능 탭에 "가상화 : 사용 안 함"으로 적혀있음.

[참고](https://zzangprogrammer.tistory.com/248)
BIOS 진입 후
Advanced(고급) > CPU구성(CPU Features) > SVM Mode(가상화설정) - 사용안함을 사용함(Disabled->Enabled)으로 변경 (Enabled(활성화)> F10으로 저장 후 재부팅> 완료

