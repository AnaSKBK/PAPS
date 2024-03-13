# Лабораторная работа 6

Тема: Использование шаблонов проектирования. <br/>
Деменева Анастасия, Шутов Артемий, Малахов Константин
# Порождающие шаблоны
## Абстрактная фабрика
Абстрактная фабрика предоставляет интерфейс для создания семейства взаимосвязанных или родственных объектов (dependent or related objects), не специфицируя их конкретных классов. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/2a50b5bf-3db1-4421-b80d-09fa85fba7c7)

Реализация:
```
from abc import ABC, abstractmethod

class AbstractFactory(ABC):
    @abstractmethod
    def create_slide(self):
        pass

    @abstractmethod
    def create_interactive_element(self):
        pass

class PresentationFactory1(AbstractFactory):
    def create_slide(self):
        return BasicSlide()

    def create_interactive_element(self):
        return Graph()

class PresentationFactory2(AbstractFactory):
    def create_slide(self):
        return TitleSlide()

    def create_interactive_element(self):
        return Animation()

class Slide(ABC):
    pass

class InteractiveElement(ABC):
    @abstractmethod
    def interact(self, slide):
        pass

class BasicSlide(Slide):
    pass

class TitleSlide(Slide):
    pass

class Graph(InteractiveElement):
    def interact(self, slide):
        print(f"Displaying graph on {slide.__class__.__name__}")

class Animation(InteractiveElement):
    def interact(self, slide):
        print(f"Showing animation on {slide.__class__.__name__}")

class Presentation:
    def __init__(self, factory: AbstractFactory):
        self.slide = factory.create_slide()
        self.interactive_element = factory.create_interactive_element()

    def display(self):
        self.interactive_element.interact(self.slide)

# Пример использования
if __name__ == "__main__":
    # Создание презентации с использованием первой фабрики
    factory1 = PresentationFactory1()
    presentation1 = Presentation(factory1)
    presentation1.display()

    # Создание презентации с использованием второй фабрики
    factory2 = PresentationFactory2()
    presentation2 = Presentation(factory2)
    presentation2.display()
```
## Строитель 
Строитель отделяет конструирование сложного объекта от его представления, так что в результате одного и того же процесса конструирования могут получаться разные представления. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/889f7de8-f894-4816-8e87-865f24b56b2a)

Реализация:
```
class Slide:
    def __init__(self):
        print("Слайд создан")
 
class InteractiveElement:
    def __init__(self):
        print("Интерактивный элемент создан")
 
class Presentation:
    def __init__(self):
        self.parts = []
 
    def add(self, part):
        self.parts.append(part)
 
class Section:
    def __init__(self):
        print("Раздел создан")
 
class Builder:
    def build_slide(self):
        pass
 
    def build_interactive_element(self):
        pass
 
    def build_section(self):
        pass
 
    def get_result(self):
        pass
 
class PresentationBuilder(Builder):
    def __init__(self):
        self.presentation = Presentation()
 
    def build_slide(self):
        self.presentation.add(Slide())
 
    def build_interactive_element(self):
        self.presentation.add(InteractiveElement())
 
    def build_section(self):
        self.presentation.add(Section())
 
    def get_result(self):
        return self.presentation
 
# Пример использования
if __name__ == "__main__":
    builder = PresentationBuilder()
    builder.build_slide()
    builder.build_interactive_element()
    builder.build_section()
    presentation = builder.get_result()
```
## Фабричный метод
Фабричный метод определяет интерфейс для создания объекта, но оставляет подклассам решение о том, какой класс инстанцировать. Фабричный метод позволяет классу делегировать инстанцирование подклассам. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/e400ad2c-639c-4fea-a26c-b935e1d996a9)

Реализация:
```
from abc import ABC, abstractmethod
 
class Product(ABC):
    pass
 
class ConcreteProductA(Product):
    def __repr__(self):
        return "ConcreteProductA"
 
class ConcreteProductB(Product):
    def __repr__(self):
        return "ConcreteProductB"
 
class Creator(ABC):
    @abstractmethod
    def factory_method(self):
        pass
 
class ConcreteCreatorA(Creator):
    def factory_method(self):
        return ConcreteProductA()
 
class ConcreteCreatorB(Creator):
    def factory_method(self):
        return ConcreteProductB()
 
if __name__ == "__main__":
    creators = [ConcreteCreatorA(), ConcreteCreatorB()]
 
    for creator in creators:
        product = creator.factory_method()
        print(f"Created {product}")
```


# Структурные шаблоны
## Адаптер
Адаптер преобразует интерфейс одного класса в интерфейс другого, который ожидают клиенты. Адаптер делает возможной совместную работу классов с несовместимыми интерфейсами. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/b21159c9-03ab-46d3-bb1a-7fa77565a4fd)

Реализация:
```
class Target:
    """Целевой класс"""
 
    def request(self):
        print("Called Target request()")
 
class Adapter(Target):
    """Адаптер"""
 
    def __init__(self):
        self.adaptee = Adaptee()
 
    def request(self):
        # Возможно выполнение какой-то другой работы
        # и затем вызов SpecificRequest
        self.adaptee.specific_request()
 
class Adaptee:
    """Адаптируемый класс"""
 
    def specific_request(self):
        print("Called SpecificRequest()")
 
if __name__ == "__main__":
    # Создаем адаптер и делаем запрос
    target = Adapter()
    target.request()
```
## Мост
Мост позволяет отделить абстракцию от реализации таким образом, чтобы и абстракцию, и реализацию можно было изменять независимо друг от друга. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/80b2d9a3-07ab-4465-9db7-6046f65522f5)

Реализация:
```
from abc import ABC, abstractmethod
 
class Client:
    def main(self):
        abstraction = RefinedAbstraction(ConcreteImplementorA())
        abstraction.operation()
        abstraction.implementor = ConcreteImplementorB()
        abstraction.operation()
 
class Abstraction(ABC):
    def __init__(self, imp):
        self._implementor = imp
 
    @property
    def implementor(self):
        return self._implementor
 
    @implementor.setter
    def implementor(self, imp):
        self._implementor = imp
 
    def operation(self):
        self._implementor.operation_imp()
 
class Implementor(ABC):
    @abstractmethod
    def operation_imp(self):
        pass
 
class RefinedAbstraction(Abstraction):
    def __init__(self, imp):
        super().__init__(imp)
 
    def operation(self):
        print("RefinedAbstraction operation:")
        self._implementor.operation_imp()
 
class ConcreteImplementorA(Implementor):
    def operation_imp(self):
        print("ConcreteImplementorA OperationImp")
 
class ConcreteImplementorB(Implementor):
    def operation_imp(self):
        print("ConcreteImplementorB OperationImp")
 
# Пример использования
if __name__ == "__main__":
    client = Client()
    client.main()
```
## Компоновщик
Компоновщик компонует объекты в древовидные структуры для представления иерархий «часть — целое». Позволяет клиентам единообразно трактовать индивидуальные и составные объекты. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/8f2423c6-9217-422b-927a-9fbb23b2947e)

Реализация:
```
from abc import ABC, abstractmethod
 
class Component(ABC):
    def __init__(self, name):
        self.name = name
 
    @abstractmethod
    def display(self):
        pass
 
    @abstractmethod
    def add(self, component):
        pass
 
    @abstractmethod
    def remove(self, component):
        pass
 
class Composite(Component):
    def __init__(self, name):
        super().__init__(name)
        self.children = []
 
    def add(self, component):
        self.children.append(component)
 
    def remove(self, component):
        self.children.remove(component)
 
    def display(self):
        print(self.name)
        for component in self.children:
            component.display()
 
class Leaf(Component):
    def __init__(self, name):
        super().__init__(name)
 
    def display(self):
        print(" " + self.name)
 
    def add(self, component):
        raise NotImplementedError("Leaf elements cannot add components.")
 
    def remove(self, component):
        raise NotImplementedError("Leaf elements cannot remove components.")
 
class Client:
    def main(self):
        root = Composite("Root")
        leaf = Leaf("Leaf")
        subtree = Composite("Subtree")
        root.add(leaf)
        root.add(subtree)
        root.display()
 
# Пример использования
if __name__ == "__main__":
    client = Client()
    client.main()
```
## Декоратор
Декторатор динамически добавляет объекту новые обязанности. Является гибкой альтернативой порождению подклассов с целью расширения функциональности. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/0e4f490c-be62-46c4-b4c2-2ae9b8ab8b05)

Реализация:
```
from abc import ABC, abstractmethod
 
class Component(ABC):
    @abstractmethod
    def operation(self):
        pass
 
class ConcreteComponent(Component):
    def operation(self):
        # Основная операция компонента
        print("Basic Operation of ConcreteComponent")
 
class Decorator(Component, ABC):
    def __init__(self, component):
        self._component = component
 
    def operation(self):
        if self._component:
            self._component.operation()
 
class ConcreteDecoratorA(Decorator):
    def operation(self):
        super().operation()
        # Дополнительная функциональность декоратора A
        print("Additional Operation of ConcreteDecoratorA")
 
class ConcreteDecoratorB(Decorator):
    def operation(self):
        super().operation()
        # Дополнительная функциональность декоратора B
        print("Additional Operation of ConcreteDecoratorB")
 
# Пример использования
if __name__ == "__main__":
    simple = ConcreteComponent()
    decoratorA = ConcreteDecoratorA(simple)
    decoratorB = ConcreteDecoratorB(decoratorA)
 
    # Применение декораторов
    print("Operation with ConcreteDecoratorA:")
    decoratorA.operation()
    print("\nOperation with ConcreteDecoratorB:")
    decoratorB.operation()
```

# Поведенческие шаблоны
## Цепочка обязанностей
Цепочка обязанностей позволяет избежать привязки отправителя запроса к его получателю, давая шанс обработать запрос нескольким объектам. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/85ba64cd-e53d-4320-bacc-52468c2e9f7b)

Реализация:
```
class InteractionReceiver:
    def __init__(self, question, video, slide):
        self.question = question
        self.video = video
        self.slide = slide
 
class InteractionHandler:
    def __init__(self):
        self.successor = None
 
    def set_successor(self, successor):
        self.successor = successor
 
    def handle(self, receiver):
        pass
 
class QuestionHandler(InteractionHandler):
    def handle(self, receiver):
        if receiver.question:
            print("Обработка вопроса от аудитории")
        elif self.successor:
            self.successor.handle(receiver)
 
class VideoHandler(InteractionHandler):
    def handle(self, receiver):
        if receiver.video:
            print("Запуск видео")
        elif self.successor:
            self.successor.handle(receiver)
 
class SlideHandler(InteractionHandler):
    def handle(self, receiver):
        if receiver.slide:
            print("Переключение слайда")
        elif self.successor:
            self.successor.handle(receiver)
 
if __name__ == "__main__":
    receiver = InteractionReceiver(question=True, video=False, slide=True)
 
    question_handler = QuestionHandler()
    video_handler = VideoHandler()
    slide_handler = SlideHandler()
 
    question_handler.set_successor(video_handler)
    video_handler.set_successor(slide_handler)
 
    question_handler.handle(receiver)
```
## Команда
Команда инкапсулирует запрос как объект, позволяя тем самым задавать параметры клиентов для обработки соответствующих запросов, ставить запросы в очередь или протоколировать их, а также поддерживать отмену операций. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/f0b7dd71-c597-416e-b6da-1a11d41ecec9)

Реализация:
```
from abc import ABC, abstractmethod
 
class Command(ABC):
    def __init__(self, receiver):
        self.receiver = receiver
 
    @abstractmethod
    def execute(self):
        pass
 
class ConcreteCommand(Command):
    def __init__(self, receiver):
        super().__init__(receiver)
 
    def execute(self):
        self.receiver.action()
 
class Receiver:
    def action(self):
        print("Выполняем действие в презентации")
 
class Invoker:
    def __init__(self):
        self._command = None
 
    def set_command(self, command):
        self._command = command
 
    def execute_command(self):
        if self._command is not None:
            self._command.execute()
 
# Пример использования
if __name__ == "__main__":
    receiver = Receiver()
    command = ConcreteCommand(receiver)
    invoker = Invoker()
 
    invoker.set_command(command)
    invoker.execute_command()
```
## Интерпретатор
Интерпретатор определяет представление грамматики для заданного языка и интерпретатор предложений этого языка. Как правило, данный шаблон проектирования применяется для часто повторяющихся операций. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/396d788e-8d68-420c-bfd6-19496d2b23ca)

Реализация:
```
class Context:
    def __init__(self, input):
        self.input = input
        self.output = 0
 
class Expression:
    def interpret(self, context):
        if len(context.input) == 0:
            return
 
        if context.input.startswith(self.nine()):
            context.output += 9 * self.multiplier()
            context.input = context.input[2:]
        elif context.input.startswith(self.four()):
            context.output += 4 * self.multiplier()
            context.input = context.input[2:]
        elif context.input.startswith(self.five()):
            context.output += 5 * self.multiplier()
            context.input = context.input[1:]
 
        while context.input.startswith(self.one()):
            context.output += 1 * self.multiplier()
            context.input = context.input[1:]
 
    def one(self):
        pass
 
    def four(self):
        pass
 
    def five(self):
        pass
 
    def nine(self):
        pass
 
    def multiplier(self):
        pass
 
class ThousandExpression(Expression):
    def one(self): return "M"
    def four(self): return " "
    def five(self): return " "
    def nine(self): return " "
    def multiplier(self): return 1000
 
class HundredExpression(Expression):
    def one(self): return "C"
    def four(self): return "CD"
    def five(self): return "D"
    def nine(self): return "CM"
    def multiplier(self): return 100
 
class TenExpression(Expression):
    def one(self): return "X"
    def four(self): return "XL"
    def five(self): return "L"
    def nine(self): return "XC"
    def multiplier(self): return 10
 
class OneExpression(Expression):
    def one(self): return "I"
    def four(self): return "IV"
    def five(self): return "V"
    def nine(self): return "IX"
    def multiplier(self): return 1
 
if __name__ == "__main__":
    roman = "MMXXIV"
    context = Context(roman)
 
    tree = [
        ThousandExpression(),
        HundredExpression(),
        TenExpression(),
        OneExpression()
    ]
 
    for exp in tree:
        exp.interpret(context)
 
    print(f"{roman} = {context.output}")
```
## Итератор
Итератор представляет доступ ко всем элементам составного объекта, не раскрывая его внутреннего представления. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/527f2965-5f49-461d-a130-f12ea0e40558)

Реализация:
```
class Aggregate:
    def create_iterator(self):
        pass
 
class ConcreteAggregate(Aggregate):
    def __init__(self):
        self._items = []
 
    def create_iterator(self):
        return ConcreteIterator(self)
 
    def __getitem__(self, index):
        return self._items[index]
 
    def __setitem__(self, index, value):
        if index >= len(self._items):
            self._items.append(value)
        else:
            self._items[index] = value
 
    def __len__(self):
        return len(self._items)
 
class Iterator:
    def first(self):
        pass
 
    def next(self):
        pass
 
    def is_done(self):
        pass
 
    def current_item(self):
        pass
 
class ConcreteIterator(Iterator):
    def __init__(self, aggregate):
        self._aggregate = aggregate
        self._current = 0
 
    def first(self):
        return self._aggregate[0]
 
    def next(self):
        self._current += 1
        if not self.is_done():
            return self._aggregate[self._current]
        else:
            return None
 
    def is_done(self):
        return self._current >= len(self._aggregate)
 
    def current_item(self):
        if not self.is_done():
            return self._aggregate[self._current]
        else:
            return None
 
# Пример использования
if __name__ == "__main__":
    a = ConcreteAggregate()
    a[0] = "Слайд A"
    a[1] = "Слайд B"
    a[2] = "Слайд C"
    a[3] = "Слайд D"
 
    iterator = a.create_iterator()
 
    print("Итерация по элементам коллекции слайдов:")
 
    item = iterator.first()
    while not iterator.is_done():
        print(item)
        item = iterator.next()
```
## Посредник
Посредник представляет такой шаблон проектирования, который обеспечивает взаимодействие множества объектов без необходимости ссылаться друг на друга. <br/>
UML-диаграмма:
![image](https://github.com/AnaSKBK/PAPS/assets/128895913/b4c8449c-f3ee-4508-b49c-5aa14e873e65)

Реализация:
```
class Mediator:
    def send(self, msg, colleague):
        pass
 
class Colleague:
    def __init__(self, mediator):
        self.mediator = mediator
 
    def send(self, message):
        self.mediator.send(message, self)
 
    def notify(self, message):
        pass
 
class CustomerColleague(Colleague):
    def notify(self, message):
        print(f"Сообщение заказчику: {message}")
 
class ProgrammerColleague(Colleague):
    def notify(self, message):
        print(f"Сообщение программисту: {message}")
 
class TesterColleague(Colleague):
    def notify(self, message):
        print(f"Сообщение тестеру: {message}")
 
class ManagerMediator(Mediator):
    def __init__(self):
        self.customer = None
        self.programmer = None
        self.tester = None
 
    def send(self, msg, colleague):
        if colleague == self.customer:
            self.programmer.notify(msg)
        elif colleague == self.programmer:
            self.tester.notify(msg)
        elif colleague == self.tester:
            self.customer.notify(msg)
 
# Пример использования
if __name__ == "__main__":
    mediator = ManagerMediator()
 
    customer = CustomerColleague(mediator)
    programmer = ProgrammerColleague(mediator)
    tester = TesterColleague(mediator)
 
    mediator.customer = customer
    mediator.programmer = programmer
    mediator.tester = tester
 
    # Процесс создания презентации
    customer.send("Нужна новая презентация на тему 'Искусственный интеллект'")
    programmer.send("Презентация создана, нужно протестировать")
    tester.send("Презентация протестирована, готова к использованию")
```
