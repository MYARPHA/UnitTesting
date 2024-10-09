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
        public void GetWeather_ReturnsCorrectWeatherFoecast()
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
        public void GetCustomerInfo_ReturnsCustomerInfo()
        {
            int customerId = 1;
        
            var customer = new Customer { Email = "test@gmail.com", Id = customerId, Name = "Cory" };
            var order = new Order { Amount = 10, Customer = customer, Date = DateTime.Now, Id = 1 };
        
            mockCustomerRepository.Setup(repo => repo.GetCustomerById(1)).Returns(customer);
            mockOrderRepository.Setup(repo => repo.GetOrders().ToString()).Returns(Convert.ToString(order));
        
            var service = new ShopService(mockCustomerRepository.Object, mockOrderRepository.Object, mockNotificationService.Object);
            var result = service.GetCustomerInfo(customerId);
        
            Assert.NotEmpty(result);
       }

       [Fact]
       public void CreateOrder_AddsOrder()
       {
           var customer = new Customer { Email = "test@icloud.com", Id = 1, Name = "Alyssa" };
           var order = new Order { Amount = 1, Customer =  customer, Date = DateTime.Now, Id = 1};
       
           mockOrderRepository.Setup(repo => repo.AddOrder(order));
       
           var service = new ShopService(mockCustomerRepository.Object, mockOrderRepository.Object, mockNotificationService.Object);
           service.CreateOrder(order);
       
           mockOrderRepository.Verify(repo => repo.AddOrder(It.IsAny<Order>()), Times.Once);
       }
    }
}


