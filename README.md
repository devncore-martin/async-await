# async-await
C# 5.0부터 새로운 C# 키워드로 async와 await가 추가되었습니다. 이 키워드들은 기존의 비동기 프로그래밍을 보다 손쉽게 지원하기 위해 C# 5.0에 추가된 중요한 기능입니다.    

C# async는 컴파일러에게 해당 메서드가 await를 가지고 있음을 알려주는 역활을 합니다. async라고 표시된 메서드는 await를 1개 이상 가질 수 있는데, 하나도 없는 경우라도 컴파일은 가능하지만 Warning 메시지가 표시된다.

await가 기다리는 Task는 대부분의 경우 Background Worker Thread에서 실행됩니다. 하지만 await를 썼다고 해서 자동으로 그 Task(혹은 메서드)가 Worker Thread에서 도는 것은 아닙니다. 만약 Worker Thread를 생성하려면, Task.Run() 등과 메서드를 사용하여 비동기 작업을 지정할 수 있습니다.

## 흐름 순서를 알아보기 위한 예제 코드

```C#
class Program
    {
        static void Main(string[] args)
        {
            Caller();

            Console.ReadLine(); // 프로그램 종료 방지
        }

        async static private void MyMethodAsync(int count)
        {
            Console.WriteLine("C");
            Console.WriteLine("D");

            await test(count);
            var a = Stadium.GetExamStadiumDetailList();
            foreach (var item in a)
            {
                Console.WriteLine("{0}테스트", item.ID);
            }

            Console.WriteLine("G");
            Console.WriteLine("H");
        }

        static async Task test(int count)
        {
            for (int i = 1; i <= count; i++)
            {
                Console.WriteLine("{0}/{1} ...", i, count);
                await Task.Delay(100); // Thread.Sleep()의 비동기 버전
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
TBD..

