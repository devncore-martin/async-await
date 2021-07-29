# async-await
C# 5.0부터 새로운 C# 키워드로 async와 await가 추가되었습니다. 이 키워드들은 기존의 비동기 프로그래밍을 보다 손쉽게 지원하기 위해 C# 5.0에 추가된 중요한 기능입니다.    

C# async는 컴파일러에게 해당 메서드가 await를 가지고 있음을 알려주는 역활을 합니다. async라고 표시된 메서드는 await를 1개 이상 가질 수 있는데, 하나도 없는 경우라도 컴파일은 가능하지만 Warning 메시지가 표시됩니다.

await가 기다리는 Task는 대부분의 경우 Background Worker Thread에서 실행됩니다. 하지만 await를 썼다고 해서 자동으로 그 Task(혹은 메서드)가 Worker Thread에서 도는 것은 아닙니다. 만약 Worker Thread를 생성하려면, Task.Run() 등과 메서드를 사용하여 비동기 작업을 지정할 수 있습니다

## 흐름 순서를 알아보기 위한 예제 코드

```C#
class Program
    {
        static void Main(string[] args)
        {
            Caller();

            Console.ReadLine(); 
        }

        async static private void MyMethodAsync(int count)
        {
            Console.WriteLine("C");
            Console.WriteLine("D");

            await test(count);
            
            Console.WriteLine("G");
            Console.WriteLine("H");
        }

        static async Task test(int count)
        {
            for (int i = 1; i <= count; i++)
            {
                Console.WriteLine("{0}/{1} ...", i, count);
                await Task.Delay(100);
            }
        }

        static void Caller()
        {
            Console.WriteLine("A");
            Console.WriteLine("B");

            MyMethodAsync(3);

            Console.WriteLine("E");
            Console.WriteLine("F");
        }
    }
```
### _출력 콘솔_

![image](https://user-images.githubusercontent.com/68521148/127269898-53585252-6939-4d90-b08b-3417482b6e6a.png)

**작업 순서**  
▪️ &nbsp; Caller() 메서드 호출    
▪️ &nbsp; A, B 출력 후 MyMethodAsync(int) 메서드 호출    
▪️ &nbsp; C, D 출력 후 test(int) 메서드 호출    
▪️ &nbsp; 1/3 출력 후 Caller() 메서드로 다시 돌아가 E, F 출력    
▪️ &nbsp; test(int) 메서드 남은 작업 2/3, 3/3 출력    
▪️ &nbsp; test(int) 메서드가 마무리 될 때 까지 기다린 후 MyMethodAsync(int) 남은 작업 G, H 출력    

