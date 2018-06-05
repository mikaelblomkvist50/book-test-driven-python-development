# Test Driven Python Development by Siddharta Govindaraj

* [Good Reads](https://www.goodreads.com/book/show/25475104-test-driven-python-development)

* [Amazon](https://www.amazon.com/Driven-Python-Development-Siddharta-Govindaraj/dp/1783987928)

# Folder Structure

# Methods covered in the book

* `assertIsNone`
* `assertEqual`
* `assertRaises` * 2
* `assertAlmostEqual`
* `fail`
* `assertTrue`
* `assertFalse` * 2
* `helper methods`

### `assertIsNone` in detail

The test creates a Stock object and then checks if the price is None. assertIsNone is a method provided by the TestCase class that we are inheriting from. It checks that its parameter is None. If the parameter is not None, it raises an AssertionError and fails the test. Otherwise, execution continues to the next line. Since that is the last line of the method, the test completes and is marked as a pass. -- *Chapter 2 - Red Green Refactor - The TDD Cycle*

```python
def test_price_of_new_stock_class_should_should_be_none(self):
      stock = Stock("GOOG")
      self.assertIsNone(stock.price)
```

### `assertEqual` in detail

```python
def test_stock_update(self):
      """An update should set the price on the stock object
      We will be using the `datetime` module for the timestamp
      """
      goog = Stock("GOOG")
      goog.update(datetime(2014, 2, 12), price=10)
      self.assertEqual(10, goog.price)
```

### `assertRaises` in detail

```python
def test_negative_price_should_throw_ValueError(self):
      goog = Stock("GOOG")
      self.assertRaises(ValueError, goog.update, datetime(2014, 2, 13), -1)

```

-- Chapter ..


```python
def test_negative_price_should_throw_ValueError(self):
      goog = Stock("GOOG")
      with self.assertRaises(ValueError):
          goog.update(datetime(2014, 2, 13), -1)
```

-- Chapter ..

### `assertAlmostEqual` in detail

The assertAlmostEqual method has two ways of specifying tolerance. The method we use below involves passing in a delta parameter which says that the difference between the expected value and the actual value should be within the delta. Which means any value between 8.3999 and 8.4001 will pass the test.

```python
def test_stock_price_should_give_the_latest_price(self):
      goog = Stock("GOOG")
      goog.update(datetime(2014, 2, 12), price=10)
      goog.update(datetime(2014, 2, 12), price=8.4)
      self.assertAlmostEqual(8.4, goog.price, delta=0.0001)
```

The other way of specifying tolerance is to use the places parameter. If this parameter is used, then both the expected and the actual values are arounded to the given number of decimal places before being compared.

```python
def test_stock_price_should_give_the_latest_price(self):
      goog = Stock("GOOG")
      goog.update(datetime(2014, 2, 12), price=10)
      goog.update(datetime(2014, 2, 12), price=8.4)
      self.assertAlmostEqual(8.4, goog.price, places=4)
```

### `fail` in detail

This call is wrapped with a try...except block to catch ValueError. If the exception is raised correctly, then control goes into the excpet block where we return from the test. Since the test method returned successfully, it is marked as passing. If the exception is not raised, then the fail method getts called. This is another methd provided by unittest. TestCase and raises a test failure exception when it is called. We can pass in a message to provide some explanation as to why it failed.

```python
def test_negative_price_should_throw_ValueError(self):
      goog = Stock("GOOG")
      try:
          goog.update(datetime(2014, 2, 13), -1)
      except ValueError:
         return
      self.fail("ValueError was not raised")
```

### `assertTrue` in detail

```python
def test_increasing_trend_is_true_if_price_increases_for_3_updates(self):
      timestamps = [datetime(2014, 2, 11), datetime(2014, 2, 12), datetime(2014, 2, 13)]
      prices = [8, 10, 12]
      for timestamp, price in zip(timestamps, prices):
          self.goog.update(timestamp, price)
      self.assertTrue(self.goog.is_increasing_trend())
```

### `assertFalse` in detail

```python
def test_increasing_trend_is_false_if_price_decreases(self):
      timestamps = [datetime(2014, 2, 11), datetime(2014, 2, 12), datetime(2014, 2, 13)]
      prices = [8, 12, 10]
      for timestamp, price in zip(timestamps, prices):
          self.goog.update(timestamp, price)
      self.assertFalse(self.goog.is_increasing_trend())
```

```python
def test_increasing_trend_is_false_if_price_equal(self):
      timestamps = [datetime(2014, 2, 11), datetime(2014, 2, 12), datetime(2014, 2, 13)]
      prices = [8, 10, 10]
      for timestamp, price in zip(timestamps, prices):
          self.goog.update(timestamp, price)
      self.assertFalse(self.goog.is_increasing_trend())
```

### `helper method` in detail

```python
def given_a_series_of_prices(self, prices):
      timestamps = [datetime(2014, 2, 11), datetime(2014, 2, 12), datetime(2014, 2, 13)]
      for timestamp, price in zip(timestamps, prices):
          self.goog.update(timestamp, price)
```

### `setUp` in detail

If you notice, each test does the same setup by instantiating a Stock object that is then used in the test. In this case, the setup is just one line, but sometimes we might have to do multiple steps before we are ready to run the test. In stead of repeating this setup code in each and every test, we can make use of the `setUp` method provided by the `TestCase` class:

### `assertEqual` vs `assertIs`

Theses two sets of assertions are very similar. The critical difference is that the former checks for equality while the latter assertion is used to checks for object identity. The second assertion fails because although both objects are equal, they are still two different objects, and hence their identity is different:

```
>>> import unittest
>>> test = unittest.TestCase()
>>> test.assertEqual([1, 2], [1, 2])
>>> test.assertIs([1, 2], [1, 2])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Users/bc/.pyenv/versions/3.6.2/lib/python3.6/unittest/case.py", line 1102, in assertIs
    self.fail(self._formatMessage(msg, standardMsg))
  File "/Users/bc/.pyenv/versions/3.6.2/lib/python3.6/unittest/case.py", line 670, in fail
    raise self.failureException(msg)
AssertionError: [1, 2] is not [1, 2]
>>>
```
