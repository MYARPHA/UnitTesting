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
