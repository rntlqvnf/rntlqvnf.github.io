간단한 노트

오늘 배운건.. 마프때 죽어라 한 내용들이라서 겁나 친숙하더라 ㅋㅋ

# Process creation

Parent - Child 의 관계로 process 를 만듬 => 트리구조가 형성

PID : process id. 보통 이걸로 관리함 (약간 고유값?)

보통 parent 가 child에게 자원과 privilege를 donation??? 이건 뭐지

아 단계별로 쉐어링

전부 쉐어 or 일부 share or no share

// A child process may be able to obtain its resources directly from the operating system, or it may be constrained to a subset of the resources of the parent process. (OS concepts 117p) , partitioning or sharing

Execution
동시에 실행
한쪽만 실행

creation에는 두가지가 있다.

creation / clone

차이? 

creation 은 코드와 데이터를 메모리에 올리고, empty stack 을 만든다.

그리고 process switch 이후의 형태로 state 를 초기화 한다.

//그럼 creation은 현재 프로세스를 안 멈추나보네.
//이거 정확한 차이를 잘 모르겠다.

cline은 현재 프로세스를 멈추고, 데이터를 복제한다. (code, data, stack, state)

UNIX 에서는 create 가 없음!
fork와 exec만!

fork는 current proccess의 copy를 만들게 한다 (그리고 실행!) (No arguments)

exec는 새로운 code/data를 덮어씌워서 해당 루틴을 돌리게 함

exit는 main 에서 return으로 implicit 하게 가능

나는 그때도 애매했는데 저거 race condition 안생기던가?

종료 후에는 그냥 남아있나봄. wait을 부르면 쫙 reaping 한다고 보는 것 같음.

왜 종료 후에도 뭔가 정보가 남아있을까?

Its entry in the process table must remain there until the parent call wait() because the process table contains the process's exit status. (PPT 에서는 PCB가 남는다고 설명)

zombie가 되면 init 가 입양해서 주기적으로 init 를 불러 수확. (리눅스는 부모를 재할당)

혹은 애초에 부모가 죽으면 다 죽이기도 함..

PCB에 저장, 스위치, 실행, PCB에 저장. 스위치, PCB로드

When an interrupt occurs, the system needs to save the current context of the process running on the CPU so that
it can restore that context when its processing is done, essentially suspending the process and then resuming it. The context is represented in the PCB of the process.

Ready Running Blocked


그림에서 CPU는 CPU와 CPU scheduler 가 함께 표현된 형태

곧, 이 scheduler 가 처리한 내용은 바로 cpu를 통해 수행되므로 그만큼 process간의 term이 짧다.. short term shedular.
얘는 selects from among the processes that are ready to execute and allocates the CPU to one of them

Ready queue는 process가 생성된 것을 한꺼번에 담고 있으므로 각각을 처리하는 term 이 크다! long term.. 
디스크에 있는 프로세스 데이터를 메모리로 끌고 옴..
degree of multiprogramming을 담당..
만약 이게 좋으면 프로세스가 새로 올라오는 속도 = 나가는 속도.
Thus, the long-term scheduler may need to be invoked only when a process leaves the system
Because of the longer interval between executions, the long-term scheduler can afford to take more time to decide
which process should be selected for execution
I/O bound는 I/O하는데 시간 더걸리는거 (토탈로) (계산보다)
CPU bound 는 반대
이 둘을 적절히 잘 섞어서 해야한다.. 안그럼 한쪽이 오버헤드 걸리니까.
If all processes are I/O bound, the ready queue will
almost always be empty, and the short-term scheduler will have little to do.
If all processes are CPU bound, the I/O waiting queue will almost always be
empty, devices will go unused, and again the system will be unbalanced.

Process간에 우선순위..

I/O와 관련된 Process가 처리되고 있는 순간에 중요한 process가 개입(I/O는 느려욧) => 중요한걸 먼저 처리! -> 그 다음으로 이어서 처리 : swapping
이게 mid term schedular
reduce the degree of multiprogamming
나중에, 다시 메모리로 들어올 수 있는데, 그때의 실행을 기억하고 있어야 함! 이게 swapping
메모리가 꽉 차면 안되니까.. 메모리 관리가 메인 목적이군!

process가 system에 들어오면
job queue(모든 process가 위치)에 넣고

메인 메모리에 상주하며 실행되길 기다리는 놈은 ready queue에..
ready queue 는 첫번째 것과 마지막 것의 PCB의 첫 주소의 포인터를 갖고 있음.

device queue.. 이건 I/O device한테 명령 내릴 queue.

# concurrency and thread!

왜 이런 아이디어가..?

비싸니까!
생성도 비싸고~ 프로세스간 소통도 비싸고!

쓰레드!

프로세스는 쓰레드와 address space로 구분..

프로세스는 어드레스 스페이스를 가지니까 드릅게 비쌈!

쓰레드는 이걸 공유.. 상대적으로 저렴!

뭐 이건 적을만한 내용이 없네