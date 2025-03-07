
* * *

### **1\. OOPs in Python - Key Concepts**

Object-Oriented Programming (OOP) is a paradigm that organizes code around objects and classes. The four main principles of OOP in Python are:

#### **1\. Class and Object**

A **class** is a blueprint for creating objects, and an **object** is an instance of a class.

`class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model

    def display_info(self):
        print(f"Car: {self.brand} {self.model}")

# Creating an object
car1 = Car("Toyota", "Camry")
car1.display_info()` 

* * *

#### **2\. Encapsulation**

Encapsulation is the bundling of data and methods that operate on the data. It restricts direct access to some variables.

`class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # Private variable

    def deposit(self, amount):
        self.__balance += amount

    def get_balance(self):
        return self.__balance

account = BankAccount(1000)
account.deposit(500)
print(account.get_balance())  # Output: 1500` 

* * *

#### **3\. Inheritance**

Inheritance allows one class to inherit properties and methods from another class.

`class Animal:
    def speak(self):
        print("Animal speaks")

class Dog(Animal):  # Dog inherits from Animal
    def speak(self):
        print("Bark!")

dog = Dog()
dog.speak()  # Output: Bark!` 

* * *

#### **4\. Polymorphism**

Polymorphism allows methods to have different behaviors based on the object.

`class Bird:
    def sound(self):
        print("Chirp!")

class Cat:
    def sound(self):
        print("Meow!")

def make_sound(animal):
    animal.sound()

make_sound(Bird())  # Output: Chirp!
make_sound(Cat())   # Output: Meow!` 

* * *

#### **5\. Abstraction**

Abstraction hides implementation details and exposes only the necessary parts.

`from abc import ABC, abstractmethod

class Shape(ABC):
 @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14 * self.radius * self.radius

circle = Circle(5)
print(circle.area())  # Output: 78.5` 

* * *
---

* * *

### **📜 Kubernetes YAML File Types and Examples**

Kubernetes uses YAML files to define various resources. Here are some of the most commonly used types:

* * *

**1\. Deployment YAML**
-----------------------

A **Deployment** manages application replicas and ensures high availability.

### **Example - `deployment.yaml`**

`apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
        ports:
        - containerPort: 80` 

📌 **Usage:** Ensures that 3 replicas of `nginx` run continuously.

* * *

**2\. Service YAML**
--------------------

A **Service** exposes a set of Pods to the network.

### **Example - `service.yaml`**

`apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP` 

📌 **Usage:** Exposes `my-app` Pods internally within the cluster.

* * *

**3\. ConfigMap YAML**
----------------------

A **ConfigMap** stores configuration data separately from the application code.

### **Example - `configmap.yaml`**

`apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  APP_ENV: "production"
  LOG_LEVEL: "debug"` 

📌 **Usage:** Provides environment variables without modifying the container image.

* * *

**4\. Secret YAML**
-------------------

A **Secret** stores sensitive information like passwords and API keys.

### **Example - `secret.yaml`**

`apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  password: c2VjcmV0MTIz  # Base64 encoded "secret123"` 

📌 **Usage:** Stores sensitive data securely.

* * *

**5\. Namespace YAML**
----------------------

A **Namespace** isolates resources within a cluster.

### **Example - `namespace.yaml`**

`apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace` 

📌 **Usage:** Creates a separate environment for workloads.

* * *

**6\. Ingress YAML**
--------------------

An **Ingress** manages external access to services using HTTP/HTTPS.

### **Example - `ingress.yaml`**

`apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80` 

📌 **Usage:** Routes external traffic to `my-service`.

* * *

**7\. Persistent Volume & Persistent Volume Claim YAML**
--------------------------------------------------------

A **Persistent Volume (PV)** provides storage, and a **Persistent Volume Claim (PVC)** requests storage.
