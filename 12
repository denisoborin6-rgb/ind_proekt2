
class Node:
    """Узел стека."""
    def __init__(self, color, count=1):
        self.color = color
        self.count = count
        self.next = None


class Stack:
    """Собственная реализация стека на основе односвязного списка."""

    def __init__(self):
        self.head = None
        self.length = 0

    def push(self, color, count=1):
        new_node = Node(color, count)
        new_node.next = self.head
        self.head = new_node
        self.length += 1

    def pop(self):
        if self.head is None:
            return None
        removed = self.head
        self.head = self.head.next
        self.length -= 1
        return removed

    def peek(self):
        return self.head

    def is_empty(self):
        return self.head is None

    def size(self):
        return self.length

    def get_top_two(self):
        if self.length < 2:
            return (None, None)

        prev = None
        curr = self.head
        while curr.next:
            prev = curr
            curr = curr.next
        return (prev, curr)

    def merge_top_two(self):
        if self.length < 2:
            return

        prev = None
        curr = self.head
        while curr.next:
            prev = curr
            curr = curr.next

        if prev:
            prev.count += curr.count
            prev.next = None
            self.length -= 1

    def clear(self):
        self.head = None
        self.length = 0


class CascadeDestroyer:
    """Алгоритм каскадного удаления цепочек."""

    def __init__(self):
        self.stack = Stack()
        self.total_removed = 0

    def process_ball(self, color):
        if self.stack.is_empty():
            self.stack.push(color, 1)
            return

        top = self.stack.peek()

        if top.color != color:
            self.stack.push(color, 1)
        else:
            top.count += 1
            if top.count >= 3:
                self._cascade_remove()

    def _cascade_remove(self):
        # Удаляем верхнюю группу (она >= 3)
        removed = self.stack.pop()
        self.total_removed += removed.count

        # Проверяем слияние двух верхних групп
        self._check_and_merge()

    def _check_and_merge(self):
        while self.stack.size() >= 2:
            bottom, top = self.stack.get_top_two()

            if bottom is None or top is None:
                break

            if bottom.color == top.color:
                # Сливаем группы
                self.stack.merge_top_two()
                top = self.stack.peek()

                # Если после слияния получилось >= 3 — удаляем
                if top and top.count >= 3:
                    removed = self.stack.pop()
                    self.total_removed += removed.count
                    # Проверяем слияние снова (рекурсивно)
                    self._check_and_merge()
                    break
                else:
                    break
            else:
                break

    def get_total_removed(self):
        return self.total_removed


def destroy_balls(balls):
    destroyer = CascadeDestroyer()
    for color in balls:
        destroyer.process_ball(color)
    return destroyer.get_total_removed()


def validate_ball_count(value):
    if not value or value.strip() == "":
        return (False, 0, "Количество не может быть пустым!")
    try:
        n = int(value.strip())
        if n < 1:
            return (False, 0, "Количество должно быть не менее 1!")
        if n > 100000:
            return (False, 0, "Количество не может превышать 100000!")
        return (True, n, "")
    except ValueError:
        return (False, 0, "Введите целое число!")


def validate_colors(value, expected_count):
    if not value or value.strip() == "":
        return (False, [], "Цвета не могут быть пустыми!")

    parts = value.strip().split()
    if len(parts) != expected_count:
        return (False, [], f"Ожидается {expected_count} чисел, введено {len(parts)}!")

    colors = []
    for i, part in enumerate(parts, 1):
        try:
            color = int(part)
            if color < 0 or color > 9:
                return (False, [], f"Цвет должен быть от 0 до 9! Ошибка в {i}-м числе")
            colors.append(color)
        except ValueError:
            return (False, [], f"Некорректное число: {part}")

    return (True, colors, "")


def run_tests():
    print("\n" + "=" * 70)
    print("  🧪 ТЕСТИРОВАНИЕ ЗАДАЧИ №12 «ШАРИКИ»")
    print("=" * 70)

    test_cases = [
        # Группа 1: Базовая функциональность
        ([1, 3, 3, 3, 2], 3, "Тройка в середине"),
        ([1, 1, 1, 2, 3], 3, "Тройка в начале"),
        ([1, 2, 3, 3, 3], 3, "Тройка в конце"),
        ([1, 1, 1, 1], 4, "Четыре шарика"),
        ([1, 2, 3, 4, 5], 0, "Нет цепочек"),
        ([1, 1, 1], 3, "Минимальная цепочка"),

        # Группа 2: Каскадное удаление
        ([3, 3, 2, 1, 1, 1, 2, 2, 3, 3], 10, "Пример из условия (каскад)"),
        ([1, 1, 2, 2, 2, 1, 1], 6, "Двойная цепочка"),
        ([1, 1, 2, 2, 2, 3, 3, 3, 1, 1], 10, "Тройной каскад"),
        ([1, 2, 2, 2, 1, 1, 1, 2, 2, 2, 1], 11, "Сложный каскад"),

        # Группа 3: Граничные случаи
        ([5], 0, "Один шарик"),
        ([1, 1], 0, "Два шарика"),
        ([9, 9, 9], 3, "Максимальный цвет (9)"),
        ([0, 0, 0], 3, "Минимальный цвет (0)"),
        ([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], 0, "Все цвета разные"),
    ]

    passed = 0
    for balls, expected, desc in test_cases:
        result = destroy_balls(balls)
        passed_test = (result == expected)
        status = "✅" if passed_test else "❌"
        if passed_test:
            passed += 1
        print(f"{status} {desc}: {balls} -> {result} (ожид. {expected})")

    # Тест производительности
    print("\n" + "-" * 70)
    print("  ⚡ ТЕСТ ПРОИЗВОДИТЕЛЬНОСТИ:")
    large = [1] * 100000
    result = destroy_balls(large)
    status = "✅" if result == 100000 else "❌"
    print(f"{status} 100000 шариков -> {result} (ожид. 100000)")
    if result == 100000:
        passed += 1

    print("\n" + "=" * 70)
    print(f"📊 ИТОГО: {passed}/{len(test_cases) + 1} тестов пройдено")
    print("=" * 70)


def print_header():
    print("\n" + "=" * 60)
    print("  🎈  ИГРА «ШАРИКИ»")
    print("  Задача №12")
    print("=" * 60)


def print_menu():
    print("\n" + "-" * 40)
    print("  📋 ГЛАВНОЕ МЕНЮ:")
    print("  1. 🎨 Ввести цепочку шариков")
    print("  2. 🧪 Запустить тестирование")
    print("  3. 🚪 Выйти из программы")
    print("-" * 40)


def process_input():
    print("\n" + "-" * 40)
    print("  📝 ВВОД ЦЕПОЧКИ")
    print("-" * 40)

    while True:
        n_input = input("  ➤ Введите количество шариков (1-100000): ").strip()
        is_valid, n, error = validate_ball_count(n_input)
        if is_valid:
            break
        print(f"  ❌ Ошибка: {error}")

    while True:
        print(f"  ➤ Введите {n} цветов (0-9) через пробел:")
        colors_input = input("  ➤ ")
        is_valid, colors, error = validate_colors(colors_input, n)
        if is_valid:
            break
        print(f"  ❌ Ошибка: {error}")

    result = destroy_balls(colors)

    print("\n" + "-" * 40)
    print("  📊 РЕЗУЛЬТАТ:")
    print(f"  🎯 Уничтожено шариков: {result}")

    if result == 0:
        print("  ℹ️  Нет цепочек из 3+ шариков")
    elif result == len(colors):
        print("  💥 Уничтожены ВСЕ шарики!")
    else:
        print(f"  📍 Осталось: {len(colors) - result}")

    print("-" * 40)
    input("\n  Нажмите Enter, чтобы продолжить...")


def main():
    print_header()

    while True:
        print_menu()
        choice = input("  ➤ Выберите действие (1-3): ").strip()

        if choice == "1":
            process_input()
        elif choice == "2":
            run_tests()
            input("\n  Нажмите Enter, чтобы продолжить...")
        elif choice == "3":
            print("\n" + "=" * 60)
            print("  👋 До свидания! Спасибо за использование программы!")
            print("=" * 60 + "\n")
            break
        else:
            print("\n  ❌ Ошибка: Неверный выбор!")
            print("  Пожалуйста, введите число от 1 до 3.")
            input("\n  Нажмите Enter, чтобы продолжить...")


if __name__ == "__main__":
    main()
