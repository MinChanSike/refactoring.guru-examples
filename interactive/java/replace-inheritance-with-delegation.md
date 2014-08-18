replace-inheritance-with-delegation:java

###

1. Создайте поле в подклассе для содержания суперкласса. На первом этапе поместите в него текущий объект.

2. Измените методы подкласса так, чтобы они использовали объект суперкласса, вместо <code>this</code>.

3. Для методов, которые были унаследованы из суперкласса и которые вызывается в клиентском коде, в подклассе нужно создать простые делегирующие методы.

4. Уберите объявление наследования из подкласса.

5. Измените код инициализации поля, в котором хранится бывший суперкласс, созданием нового объекта.



###

```
class Engine {
  //...
  private double fuel;
  private double CV;

  public double getFuel() {
    return fuel;
  }
  public void setFuel(double fuel) {
    this.fuel = fuel;
  }
  public double getCV() {
    return CV;
  }
  public void setCV(double cv) {
    this.CV = cv;
  }
}

class Car extends Engine {
  // ...
  private String brand;
  private String model;

  public String getName() {
    return brand . ' ' . model . ' (' . getCV() . 'CV)';
  }
  public String getModel() {
    return model;
  }
  public void setModel(String model) {
    this.model = $model;
  }
  public String getBrand() {
    return brand;
  }
  public void setBrand(String brand) {
    this.brand = brand;
  }
}
```

###

```
class Engine {
  //...
  private double fuel;
  private double CV;

  public double getFuel() {
    return fuel;
  }
  public void setFuel(double fuel) {
    this.fuel = fuel;
  }
  public double getCV() {
    return CV;
  }
  public void setCV(double cv) {
    this.CV = cv;
  }
}

class Car {
  // ...
  private String brand;
  private String model;
  protected Engine engine;

  public Car() {
    this.engine = new Engine();
  }
  public String getName() {
    return brand . ' ' . model . ' (' . engine.getCV() . 'CV)';
  }
  public String getModel() {
    return model;
  }
  public void setModel(String model) {
    this.model = $model;
  }
  public String getBrand() {
    return brand;
  }
  public void setBrand(String brand) {
    this.brand = brand;
  }
}
```

###

Set step 1

# Рассмотрим рефакторинг на примере класса автомобилей <code>Car</code>, который наследуется от класса двигателей <code>Engine</code>.

Select "getCV()" in "Car"

# Сперва идея наследования казалась хорошей и оправданной, но в итоге выяснилось, что автомобили используют только одно свойство двигателя (а именно, объем).

Go to the start of "Car"

# Таким образом, было бы эффективней использовать делегирование к классу <code>Engine</code> для получения нужных свойств или методов.

# Начнём рефакторинг с создания поля для хранения ссылки на суперкласс.

Go to "String model;|||"

Print:
```

  protected Engine engine;
```

# Пока что будем заполнять это поле текущим объектом (это можно сделать в конструкторе).

Go to before "getName"

Print:
```

  public Car() {
    this.engine = this;
  }
```

Set step 2

Select "getCV()" in "Car"

# Теперь следует изменить все обращения к полям и методам суперкласса так, чтобы они обращались к созданному полю. В нашем случае, это происходит только в одном месте.

Print "engine.getCV()"

Set step 4

Select " extends Engine"

# Теперь можно убрать объявление наследование из класса <code>Car</code>.

Remove selected

Set step 5

# После этого остаётся только создавать новый объект двигателей для заполнения поля связанного объекта.

Select "engine = |||this|||"

Replace "new Engine()"

#C Запускаем финальное тестирование.

#S Отлично, все работает!

Set final step

#Q На этом рефакторинг можно считать оконченным. В завершение, можете посмотреть разницу между старым и новым кодом.