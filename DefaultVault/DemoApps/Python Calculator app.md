Create a simple calculator Python file and name it as app.py

```
def add(x, y):
    return x + y

def subtract(x, y):
    return x - y

def multiply(x, y):
    return x * y

def divide(x, y):
    if y == 0:
        return "Error: Division by zero"
    return x / y

def main():
    print("Simple Calculator")
    print("Select operation:")
    print("1. Add")
    print("2. Subtract")
    print("3. Multiply")
    print("4. Divide")

    choice = input("Enter choice (1/2/3/4): ")

    if choice in ['1', '2', '3', '4']:
        num1 = float(input("Enter first number: "))
        num2 = float(input("Enter second number: "))

        if choice == '1':
            print(f"{num1} + {num2} = {add(num1, num2)}")
        elif choice == '2':
            print(f"{num1} - {num2} = {subtract(num1, num2)}")
        elif choice == '3':
            print(f"{num1} * {num2} = {multiply(num1, num2)}")
        elif choice == '4':
            print(f"{num1} / {num2} = {divide(num1, num2)}")
    else:
        print("Invalid input")

if __name__ == "__main__":
    main()
```


In the Dockerfile, define all the dependencies

```
# Using the official Alpine Linux base image

FROM alpine:latest

# Installing Python and necessary packages

RUN apk add --no-cache python3 py3-pip

# Setting the working directory in the container

WORKDIR /app

# Copying the Python script into the container

COPY app.py .

# Setting the default command to run the Python script inside bash

CMD ["bash"]
```


Ensure that the Dockerfile, Python file are in same directory. Now we will use the build command to build our images.

```
docker build -t my-app -f Dockerfile.txt .
```


Now to get the status of the container use docker ps and find command. Here the name of the image is my-app and name of the container is my-python-app.


`docker ps -a| find "my-python-app"
list`



Use docker run and at the end provide sh so that you can use the Alpine Linux inside your container. Once your are inside the container execute the python file using Python3 command

```
docker run -it --name my-python-app my-app sh
```
