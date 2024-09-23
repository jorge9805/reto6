# reto6

adding exceptions to reto1:
for the first problem I handled the division by zero and making sure that the operation introduced was into the four, for the second problem "palindromo" The input of the function has to be a string if not trhows an error, for the third problem "primos"; I ensure that every number in the list was intger and greater than one, the fourth problem I made sure that the input was a list and had at least two elements, and for the fifth problem i raise the error if the input was not a list or if some elements of the list were not strings

```
  # Función operaciones básicas (suma, resta, multiplicación, división)
  # Recibe dos números y un operador. Se maneja con excepciones para errores comunes como división por cero o un operador no válido.
  def operaciones_basicas(a, b, operacion):
      try:
          if operacion == "+":
              return a + b
          elif operacion == "-":
              return a - b
          elif operacion == "x":
              return a * b
          elif operacion == "/":
              if b == 0:
                  raise ZeroDivisionError("No se puede dividir por cero.")
              return a / b
          else:
              raise ValueError("Operación no válida. Usa '+', '-', 'x' o '/'.")
      except (TypeError, ValueError) as e:
          return f"Error: {e}"
  
  # Probando la función operaciones_basicas
  print(operaciones_basicas(2, 3, "+"))
  print(operaciones_basicas(2, 3, "-"))
  print(operaciones_basicas(2, 3, "x"))
  print(operaciones_basicas(2, 0, "/"))
  print(operaciones_basicas(2, 3, "="))
  
  
  # Función palíndromo
  # Se asegura de manejar strings y lanza una excepción si el valor no es una cadena.
  def palindromo(palabra):
      try:
          if not isinstance(palabra, str):
              raise TypeError("El valor debe ser una cadena.")
          palabra = palabra.lower().replace(" ", "")
          for i in range(len(palabra)):
              if palabra[i] != palabra[-i-1]:
                  return False
          return True
      except Exception as e:
          return f"Error: {e}"
  
  # Probando la función palíndromo
  print(palindromo("Anita lava la tina"))
  print(palindromo(123))  # Error esperado
  
  
  # Función que retorna una lista con los números primos de una lista de números
  # Maneja excepciones para valores no enteros o fuera de rango.
  from math import sqrt
  def primos(numeros):
      try:
          primos = []
          for num in numeros:
              if not isinstance(num, int) or num < 1:
                  raise ValueError(f"{num} no es un número entero válido o es menor que 1.")
              raiz = sqrt(num)
              entero_raiz = int(raiz)
              esPrimo = True
              for i in range(2, entero_raiz + 1):
                  if num % i == 0:
                      esPrimo = False
              if num == 1:
                  esPrimo = False
              if esPrimo:
                  primos.append(num)
          return primos
      except Exception as e:
          return f"Error: {e}"
  
  # Probando la función primos
  print(primos([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20]))
  print(primos([1, 2, "a", 3, -5]))  # Error esperado
  
  
  # Función mayor suma de dos elementos consecutivos
  # Se asegura que la lista sea válida y maneja listas vacías o con un solo elemento.
  def mayor_suma(numeros):
      try:
          if not isinstance(numeros, list) or len(numeros) < 2:
              raise ValueError("Se necesita una lista con al menos dos elementos.")
          mayor = numeros[0] + numeros[1]
          for i in range(len(numeros) - 1):
              suma = numeros[i] + numeros[i + 1]
              if suma > mayor:
                  mayor = suma
          return mayor
      except Exception as e:
          return f"Error: {e}"
  
  # Probando la función mayor_suma
  print(mayor_suma([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 20, 17, 18, 11]))
  print(mayor_suma([10]))  # Error esperado
  
  
  # Función que determina si palabras en una lista tienen los mismos caracteres (anagramas)
  # Se asegura de manejar listas y cadenas, y lanza excepción si los valores no son cadenas.
  def mismo_caracteres(lista):
      try:
          if not isinstance(lista, list):
              raise TypeError("Se esperaba una lista.")
          for palabra in lista:
              if not isinstance(palabra, str):
                  raise ValueError(f"'{palabra}' no es una cadena.")
          iguales = []
          for palabra in lista:
              for palabra2 in lista:
                  if palabra != palabra2:
                      if sorted(palabra) == sorted(palabra2):
                          iguales.append(palabra)
          return iguales
      except Exception as e:
          return f"Error: {e}"
  
  # Probando la función mismo_caracteres
  print(mismo_caracteres(["roma", "amor", "omar", "ramo", "hola"]))
  print(mismo_caracteres([1, 2, "hola"]))  # Error esperado
```
# Error handling in shape

For shape we handle the errors verifying that every input is correct so in point they have to be floats , in lines they have to be point, for shape the input has to be a list of lines and a list of point, now for triangle we have to ensure that is just 3 lines, and computing the area of isoceles it involves a square root so the number inside has to be positive.
```
from math import sqrt

class Point:
    def __init__(self, x, y):
        if not (isinstance(x, (int, float)) and isinstance(y, (int, float))):
            raise ValueError("Coordinates must be numeric.")
        self.x = x
        self.y = y

    def compute_distance(self, point):
        # Ensure the input is a Point instance
        if not isinstance(point, Point):
            raise TypeError("Argument must be a Point instance.")
        return sqrt((point.x - self.x)**2 + (point.y - self.y)**2)


class Line:
    def __init__(self, start: Point, end: Point):
        # Ensure start and end are instances of Point
        if not (isinstance(start, Point) and isinstance(end, Point)):
            raise TypeError("Start and end points must be of type 'Point'.")
        self.start = start
        self.end = end
        self.length = start.compute_distance(end)


class Shape:
    def __init__(self, lines, points):
        # Ensure lines and points are lists and have correct types
        if not isinstance(lines, list) or not all(isinstance(line, Line) for line in lines):
            raise TypeError("Lines must be a list of 'Line' instances.")
        if not isinstance(points, list) or not all(isinstance(point, Point) for point in points):
            raise TypeError("Points must be a list of 'Point' instances.")

        self.lines = lines
        self.points = points
        self.is_regular = self.compute_regular()

    def compute_regular(self):
        # Check if all sides are equal
        for i in range(len(self.lines) - 1):
            if self.lines[i].length != self.lines[i + 1].length:
                return False
        return True

    def vertices(self):
        return len(self.points)

    def edges(self):
        return len(self.lines)

    def inner_angles(self):
        raise NotImplementedError("Subclass must implement this method")

    def compute_area(self):
        raise NotImplementedError("Subclass must implement this method")

    def compute_perimeter(self):
        raise NotImplementedError("Subclass must implement this method")


class Triangle(Shape):
    def __init__(self, lines, points):
        # A triangle must have exactly 3 lines and 3 points
        if len(lines) != 3 or len(points) != 3:
            raise ValueError("A triangle must have exactly 3 lines and 3 points.")
        super().__init__(lines, points)
        self.isTriangle = self.compute_triangle()

    def compute_triangle(self):
        # Check if the sides form a valid triangle using the triangle inequality theorem
        if (self.lines[0].length + self.lines[1].length > self.lines[2].length and
            self.lines[0].length + self.lines[2].length > self.lines[1].length and
            self.lines[1].length + self.lines[2].length > self.lines[0].length):
            return True
        return False

    def inner_angles(self):
        return 180

    def compute_area(self):
        # Heron's formula for triangle area
        return 0.5 * abs((self.points[0].x * (self.points[1].y - self.points[2].y) +
                          self.points[1].x * (self.points[2].y - self.points[0].y) +
                          self.points[2].x * (self.points[0].y - self.points[1].y)))

    def compute_perimeter(self):
        return self.lines[0].length + self.lines[1].length + self.lines[2].length


class Equilateral(Triangle):
    def __init__(self, lines, points):
        super().__init__(lines, points)

    def inner_angles(self):
        return 60

    def compute_area(self):
        return (sqrt(3) / 4) * self.lines[0].length**2

    def compute_perimeter(self):
        return 3 * self.lines[0].length


class Isosceles(Triangle):
    def __init__(self, lines, points):
        super().__init__(lines, points)

    def inner_angles(self):
        return 90

    def compute_area(self):
        # Exception for when math results in an invalid square root (e.g., negative number)
        try:
            return 0.5 * self.lines[0].length * sqrt(self.lines[1].length**2 - (self.lines[0].length / 2)**2)
        except ValueError:
            raise ValueError("Invalid side lengths for an isosceles triangle.")

    def compute_perimeter(self):
        return self.lines[0].length + self.lines[1].length * 2


class Scalene(Triangle):
    def __init__(self, lines, points):
        super().__init__(lines, points)

    def inner_angles(self):
        return 90

    def compute_area(self):
        # Heron's formula for the area of a scalene triangle
        s = self.compute_perimeter() / 2
        return sqrt(s * (s - self.lines[0].length) * (s - self.lines[1].length) * (s - self.lines[2].length))

    def compute_perimeter(self):
        return self.lines[0].length + self.lines[1].length + self.lines[2].length


class TriRectangle(Triangle):
    def __init__(self, lines, points):
        super().__init__(lines, points)

    def inner_angles(self):
        return 90

    def compute_area(self):
        return 0.5 * self.lines[0].length * self.lines[1].length

    def compute_perimeter(self):
        return self.lines[0].length + self.lines[1].length + self.lines[2].length


class Rectangle(Shape):
    def __init__(self, lines, points):
        if len(lines) != 4 or len(points) != 4:
            raise ValueError("A rectangle must have exactly 4 lines and 4 points.")
        super().__init__(lines, points)

    def inner_angles(self):
        return 90

    def compute_area(self):
        return self.lines[0].length * self.lines[1].length

    def compute_perimeter(self):
        return 2 * (self.lines[0].length + self.lines[1].length)


class Square(Rectangle):
    def __init__(self, lines, points):
        super().__init__(lines, points)

    def inner_angles(self):
        return 90

    def compute_area(self):
        return self.lines[0].length**2

    def compute_perimeter(self):
        return 4 * self.lines[0].length


    
