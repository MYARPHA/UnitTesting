# UnitTesting

 private readonly BasicCalc _calculator;

 public Basic()
 {
     _calculator = new BasicCalc();
 }

 [Fact]
 public void Log_test()
 {
     double result = _calculator.Log(9, 3);
     Assert.Equal(2, result);
 }

 [Theory]
 [InlineData(9, 3, 2)]
 [InlineData(25, 5, 2)]
 [InlineData(100, 10, 2)]
 public void Log_Theory(int a, int b, int expectedResult)
 {
     double result = _calculator.Log(a, b);
     Assert.Equal(expectedResult, result);
 }


 [Theory]
 [InlineData(-1, 0, 2)]
 [InlineData(0, 0, 2)]
 public void Log_Exception(int a, int b, int expectedResult)
 {
     Assert.Throws<ArgumentOutOfRangeException>(() => _calculator.Log(a, b));
 }


using Moq;
using TestingLib.Shop;
using TestingLib.Weather;

namespace UnitTesting.MYARPHA
{
    public class Basic
    {
        public static Mock<IWeatherForecastSource> mockWeatherForecastSource;
        public static Mock<INotificationService> mockNotificationServies;
        public static Mock<ICustomerRepository> mockCustoerRepository;
        public static Mock<IOrderRepository> mockOrderRepository;

        public Basic()
        {
            mockWeatherForecastSource = new Mock<IWeatherForecastSource>();
            mockNotificationServies = new Mock<INotificationService>();
            mockCustoerRepository = new Mock<ICustomerRepository>();
            mockOrderRepository = new Mock<IOrderRepository>();

        }

        [Fact]
        public void GetWeather()
        {
            var timeNow = DateTime.Now;
            var forecast = new WeatherForecast
            {
                Date = timeNow,
                Summary = "Холодно жесть!!",
                TemperatureC = 44
            };

            mockWeatherForecastSource.Setup(repo => repo.GetForecast(timeNow)).Returns(forecast);

            var service = new WeatherForecastService(mockWeatherForecastSource.Object);
            var result = service.GetWeatherForecast(timeNow);

            Assert.Equal(result, forecast);
        }

        [Fact]
        public void GetCustomerInfo() 
        {
            var customer = new Customer;
            var order = new Order { };
        }
    }
}
