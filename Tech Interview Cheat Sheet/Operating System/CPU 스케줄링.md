# CPU 스케줄링

## 정의
CPU 이용률을 극대화하기 위해서는 멀티프로그래밍(multiprogramming)이 필요하다. 
하지만 만약 CPU core가 하나라면 한 번에 하나의 프로세스만 실행 가능할 것이다. 이때 필요한 것이 **CPU 스케줄링**이다. 

>즉, CPU 스케줄링은 **언제 어떤 프로세스에 CPU를 할당할지 결정하는 작업**이라고 할 수 있다. 

## 선점 / 비선점 스케줄링
- **선점 (preemptive)** : OS가 CPU의 사용권을 선점할 수 있는 경우, 강제 회수하는 경우 (처리시간 예측 어려움)
- **비선점 (nonpreemptive)** : 프로세스 종료 or I/O 등의 이벤트가 있을 때까지 실행 보장 (처리시간 예측 용이함)

>아래의 1번과 4번 상황에서만 스케줄링이 발생하는 것을 비선점형(non-preemptive) 스케줄링이라고 하고, 이외의 모든 스케줄링은 선점형(preemptive) 스케줄링이라고 한다. 
1. 실행(running) 상태에서 대기(waiting) 상태로 전환(switching)될 때 ( ex : I/O 발생 )
2. 실행(running) 상태에서 준비(ready) 상태로 전환(switching)될 때 ( ex : intterupt 발생 )
3. 대기(waiting) 상태에서 준비(ready) 상태로 전환(switching)될 때 ( ex : I/O 완료 시 )
4. 종료(Terminated)될 때  

### 선점형 스케줄링
- 프로세스가 CPU를 할당받아 실행 중이더라도 운영체제가 이를 강제로 뺐을 수 있는 방식
- CPU 처리 시간이 매우 긴 프로세스의 CPU 사용 독점을 막을 수 있어 효율적인 운영 가능
- 잦은 문맥 교환으로 오버헤드(Overhead)가 커질 수 있다. 

### 비선점형 스케줄링
- 프로세스가 CPU를 점유하고 있다면 이를 뺐을 수 없는 방식
- 필요한 문맥 교환만 일어나므로 오버헤드(overhead)가 상대적으로 적지만 프로세스의 배치에 따라 효율성 차이가 많이 난다.


## CPU 스케줄링 평가 기준 ( CPU Scheduling Criteria )
### 1. CPU 이용률(CPU utilization)  
시간당 CPU를 사용한 시간의 비율
프로세서를 실행상태로 항상 유지하려고 해야 한다.
### 2. 처리율(Throughput)  
시간당 처리한 작업의 비율
단위 시간당 완료되는 작업 수가 많도록 해야 한다.
### 3. 반환시간(Turnaround Time)   
프로세스가 생성된 후 종료되어 사용하던 자원을 모두 반환하는 데까지 걸리는 시간
작업이 준비 큐(ready queue)에서 기다린 시간부터 CPU에서 실행된 시간, I/O 작업 시간의 합이다.
### 4. 대기시간(Waiting Time)  
대기열에 들어와 CPU를 할당받기 까지 기다린 시간
준비 큐에서 기다린 시간의 총합
### 5. 반응시간(Response Time)  
대기열에서 처음으로 CPU를 얻을 때까지 걸린 시간
대기시간과 비슷하지만 다른 점은, 대기시간은 준비 큐에서 기다린 모든 시간을 합친 것이지만 반응 시간은 CPU를 할당받은 최초의 순간까지 기다린 시간 한번 만을 측정한다.  

**CPU 이용률과 처리율은 극대화하는 것**이 좋고 **반환시간, 대기시간, 반응시간은 줄이는 것**이 일반적으로 좋다.

## CPU 스케줄링 알고리즘(CPU Scheduling Algorithm)

### **FCFS (First Come First Served) 스케줄링**
- 가장 먼저 요청한 프로세스에 CPU를 할당해주는 방식이다.
- 비선점형(Non-preemptive) 스케줄링이다.
- 작성이 간단하고 이해하기 쉽다.
- 평균 대기 시간(Average Waiting Time)이 길어질 수 있다.
- 응답 시간(Response Time)이 길어질 수 있다.
- 반환시간(Turnaround Time) 면에서는 좋을 수 있다.
- Convoy Effect(호위 효과)가 발생할 수 있다.  
> Convoy Effect(호위 효과) : CPU 사용시간이 긴 프로세스에 의해 사용시간이 짧은 프로세스들이 오래 기다리는 현상이다. 이로 인해 평균 대기시간이 길어지게 된다.[추가 설명](https://ursidae.tistory.com/31)  

 ![FCFS](https://user-images.githubusercontent.com/70997596/215697931-dcf7cd3d-2b6a-47dd-8b16-a39e23bb4415.png)


P1, P2, P3 모두 0초에 순서대로 도착했다고 하면 위와 같은 결과가 나온다. 

### **SJF(Shortest-Job-First) 스케줄링**
- 다음 CPU burst time의 길이를 고려해서 스케줄링을 결정하는 알고리즘이다.  
- 비선점형과 선점형이 따로 존재한다.   
- 비선점형에서는 실행되고 있는 프로세스는 끝까지 실행한다.  
- 선점형에서는 현재 실행되고 있는 프로세스의 남은 시간보다 도착한 다음 프로세스가 더 빨리 끝날 수 있는 프로세스라면 다음 프로세스를 실행하도록 바꾸게 된다. **SRTF**(Shortest Remaining Time First)라고도 부른다. 
- SJF 스케줄링은 평균 대기 시간을 줄일 수 있다.
- 하지만 다음 프로세스의 CPU burst time을 예측하는 것이 어렵다는 문제가 존재한다.  

#### **비선점형 SJF 예시**
![SJF_비선점](https://user-images.githubusercontent.com/70997596/215698049-cf49e55a-66c2-4982-ab37-515eddd21258.png)


#### **선점형 SJF 예시**
 ![SJF_선점](https://user-images.githubusercontent.com/70997596/215698074-a99328d0-23c6-4f92-8ef8-2bb6bdb88dec.png)


선점형 SJF의 경우 실행 중이던 프로세스보다 도착한 프로세스의 남은 시간이 짧으면 해당 프로세스로 전환된다.
### **Priority 스케줄링**
- 각각의 프로세스에 우선순위 넘버가 있다.
- 가장 높은 우선순위의 프로세스에 CPU를 할당한다.
- 선점형과 비선점형이 나뉜다.
- SJF도 Priority 스케줄링이라고 할 수 있다. ( 다음 CPU burst time이 우선순위인 것처럼 작동하므로)
- 기아(Starvation) 문제가 발생할 수 있다. 기아 문제란 낮은 우선순위의 프로세스가 절대 실행되지 않는 문제를 뜻함.
- 기아문제를 해결하기 위해서 노화(aging)를 사용할 수 있다. 시간이 지날수록 프로세스의 우선순위를 높여주는 식.

![Priority](https://user-images.githubusercontent.com/70997596/215698137-343bdbd6-2688-419b-b912-5706da5c118a.png)


모두 0초에 도착했다고 했을 때, 우선순위에 따라 위와 같이 처리된다.

### **Round Robin(RR) 스케줄링**

- 각각의 프로세스에 동일한 CPU 할당 시간을 부여해서 해당 시간 동안만 CPU를 이용하게 한다.
- 할당 시간 내에 처리를 완료하지 못하면 다음 프로세스로 넘어가므로 선점형 방식이다.
- n개의 프로세스가 있을 때 할당 시간을 q로 설정하면, 어떤 프로세스도 (n-1)q 시간 이상을 기다리지 않아도 된다.
- 응답 시간을 빠르게 할 수 있다는 장점이 있다.
- q가 커진다면 FCFS처럼 작동한다. 
- q가 매우 작아지면 process sharing이라고 부른다. 이것은 n개의 프로세스가 프로세서 속도의 1/n 씩으로 작동함을 의미한다.

![RR](https://user-images.githubusercontent.com/70997596/215698153-67325861-aa04-4805-85c1-526636144899.png)


모두 0초에 도착했고, 할당 시간 q가 20 초라고 가정하면 위와 같은 결과가 나온다.



### **Multilevel Queue**

- 준비 큐가 여러 개의 큐들로 나뉜다.
- 각 큐는 각자의 스케줄링 알고리즘을 가지고 있다. 
- 각 큐 사이에서 프로세스들이 이동할 수 없다. 
- 일반적으로 Foreground 프로세스들은 Round Robin 방식을 사용하고, Background 프로세스는 FCFS를 사용한다.
- 기아(Starvation) 문제가 발생할 수 있다.
- 보통 CPU 시간의 80%는 Foreground의 RR, 20%는 Background의 FCFS에 할당된다.
![Multilevel_Queue](https://user-images.githubusercontent.com/70997596/215698201-903e2cdf-d31d-4ca2-963a-0524d23954b6.png)

### **Multilevel Feedback Queue(MFQ)**

 ![MFQ](https://user-images.githubusercontent.com/70997596/215698214-383241e1-ce23-4bad-a4e8-60f135be4626.png)

Multilevel Queue와 비슷하지만, MFQ는 각 큐 간에 프로세스들이 이동할 수 있다. 

## 출처
https://code-lab1.tistory.com/45  
