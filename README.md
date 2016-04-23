# osisp4
Создать дерево процессов согласно варианта индивидуального задания. 

Процессы непрерывно обмениваются сигналами согласно табл. 2.<br/>
Запись в таблице 1 вида:  1->(2,3,4,5) означает,
что исходный процесс 0 создаёт дочерний процесс 1,
который, в свою очередь, создаёт дочерние процессы 2,3,4,5.<br />
Запись в таблице 2 вида:  1->(2,3,4) SIGUSR1 означает, что процесс 1 посылает  дочерним процессам  2,3,4 одновременно (т.е. за один вызов kill() ) сигнал SIGUSR1.<br />
<p>Каждый процесс при получении или посылке сигнала выводит на консоль информацию в следующем виде:<br />
**N pid    ppid   послал/получил  USR1/USR2 текущее время (мксек)**<br />
где N-номер сына по табл. 1.<p/>
<p>*Процесс 1*, после получения  **101–го** по счету сигнала SIGUSR,  посылает сыновьям сигнал SIGTERM и ожидает
завершения всех сыновей, после чего завершается сам.<br />
*Процесс 0* ожидает завершения работы процесса 1 после чего завершается сам.<br />
Сыновья, получив сигнал SIGTERM, завершают работу с выводом на консоль сообщения вида:
pid    ppid   завершил работу после X-го сигнала SIGUSR1 и Y-го сигнала SIGUSR2
где X,Y – количество посланных за все время работы данным сыном сигналов SIGUSR1 и SIGUSR2.

Таблица 1

№	| Дерево процессов
------------ | -------------
1	| 1->2  2->(3,4)  4->5  3->6  6->7  7->8
**2**	| **1->(2,3,4)   2->(5,6)   6->7  7->8**
3	| 1->(2,3,4,5)   2->6   3->7  4->8
4	| 1->(2,3)   2->(4,5)   5->6    6->(7,8)  
5	| 1->(2,3,4,5)   5->(6,7,8)
6	| 1->(2,3)   3->4   4->(5,6,7)   7->8  
7	| 1->2   2->(3,4)   4->5    3->6  6->7  7->8
8	| 1->(2,3,4,5,6)   6->(7,8)   
9	| 1->2   2->(3,4,5)   4->6    3->7  5->8
10	| 1->2   2->3   3->(4,5,6)    6->7  4->8
11	| 1->(2,3)   3->4   4->(5,6) 6-7   7->8  
12	| 1->2   2->(3,4)   4->5    3->6  6->7  7->8
13	| 1->(2,3,4,5) 5->(6,7)   7->8   
14	| 1->2   2->(3,4,5)   4->6    3->7  5->8
15	| 1->2   2->3   3->(4,5,6)    6->7  4->8
16	| 1->(2,3,4,5)   2->(6,7)   7->8


Таблица 2

№ |	Последовательность обмена сигналами
------------ | -------------
1 |	1->2 SIGUSR1   2->(3,4) SIGUSR2   4->5 SIGUSR1 3->6 SIGUSR1  6->7 SIGUSR1  7->8 SIGUSR2   8->1 SIGUSR2
2 | 1->(2,3,4) SIGUSR1   2->(5,6) SIGUSR2   6->7 SIGUSR1  7->8 SIGUSR1  8->1 SIGUSR2
3 |	1->(2,3,4,5) SIGUSR2  2->6 SIGUSR1   3->7 SIGUSR1  4->8 SIGUSR1  8->1 SIGUSR1
4 |	1->(2,3) SIGUSR1   2->(4,5) SIGUSR1   5->6 SIGUSR1 6->(7,8) SIGUSR1   8->1 SIGUSR1 
5	| 1->(2,3,4,5) SIGUSR1   5->(6,7,8) SIGUSR1 8->1 SIGUSR1
6	| 1->(2,3) SIGUSR1   3->4 SIGUSR2   4->(5,6,7) SIGUSR1 7->8 SIGUSR1   8->1 SIGUSR2
7	| 1->2 SIGUSR1   2->(3,4) SIGUSR2   4->5 SIGUSR1 3->6 SIGUSR1  6->7 SIGUSR1  7->8 SIGUSR1  8->1 SIGUSR1
8	| 1->(2,3,4,5,6) SIGUSR2   6->(7,8) SIGUSR1   8->1 SIGUSR2
9	| 1->2 SIGUSR2   2->(3,4,5) SIGUSR1   4->6 SIGUSR1 3->7 SIGUSR1  5->8 SIGUSR1 8->1 SIGUSR2
**10** | **1->(8,7,6) SIGUSR1   8->4 SIGUSR1  7->4SIGUSR2 6->4 SIGUSR1  4->(3,2) SIGUSR1 2->1 SIGUSR2**
11 |	1->(8,7) SIGUSR1   8->(6,5) SIGUSR1  5->(4,3,2) SIGUSR2 2->1 SIGUSR2
12	| 1->(8,7,6,5) SIGUSR1   8->3 SIGUSR1  7->3 SIGUSR2 6->3 SIGUSR1  5->3 SIGUSR1 3->2SIGUSR2 2->1 SIGUSR2
13	| 1->6 SIGUSR1   6->7 SIGUSR1  7->(4,5) SIGUSR2 4->8 SIGUSR1  5->2 SIGUSR1 8->2 SIGUSR2 2->1 SIGUSR2
14	| 1->8 SIGUSR1   8->7 SIGUSR1  7->(4,5,6) SIGUSR2 4->2 SIGUSR1  2->3 SIGUSR1 3->1 SIGUSR2
