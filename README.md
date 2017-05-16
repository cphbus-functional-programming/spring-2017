# General
This repository contains presentation PDFs and general information about the course.

## Study points
To attend the exam, you **need at least 16** (~80% of the total number) study points. The study points are listed here:
[Study points at Google Drive](https://docs.google.com/spreadsheets/d/1SAE1xZdOu8FmvlilVS5J2CjYqTFjEbWsyA9dQBhX5KE/edit?usp=sharing)

## Reading material
Haskell: [Learn you a Haskell for great good!](http://learnyouahaskell.com/chapters). Please read chapter 1-3

# Assignments
The course assignments are listed here (corresponding to the [Google drive sheet](https://docs.google.com/spreadsheets/d/1SAE1xZdOu8FmvlilVS5J2CjYqTFjEbWsyA9dQBhX5KE/edit?usp=sharing)). The once marked in bold are the four assignments you can choose to present at the exam (see below). All assignments gives one study point.

| Assignment number | Theme |
| ---- | ---- |
| 1 | RPN (reverse polish notation) stack-based calculator |
| 2 | Virtual CPU (without tail recursion) |
| 3 | PI in Lisp |
| 4 | **Map and flatmap** in Lisp |
| 5 | Elm REST | 
| 6 | **Elm Websocket** chat *client* application |	
| 7 | **Tail recursion** in your virtual CPU |
| 8 | **Haskell webserver** with support for the chat protocol in assignment 6 |

# Exam 
You will be examinated orally. You will have to present one of the four major assignments (marked in bold above). So out of the four assignments, you have to pick one. From there we will have a discussion about the course content. You have **10 minutes** to present your assignment. Then we will ask you questions for around 7-10 minutes. We will naturally ask into other topics, but the presentation will be the starting point.

The questions for the exams are published and can be found here: [https://github.com/cphbus-functional-programming/general/blob/master/exam.md](https://github.com/cphbus-functional-programming/general/blob/master/exam.md)


# Lisp tips and tricks
**Floating point overflow**: This error occurs when a number cannot contain enough
decimals. In the pi example we need 10.000 decimals, which is way more than a normal
float of 8 bytes can handle. Instead we need a biiiig number to be able to store 10.000 decimals.
Just like in Java, Lisp has different number types, and if you get a floating
point overflow, you need to coerce Lisp to use the very precise data type.
So when you call a function for the first time, you should append 'L0'
to your numbers. This will force Lisp to use the 'long-float datatype for
the remainder of your program.

Taking the pi exercise as an example, your call to your pi function could look like this:
   (myPi 1L0 (/ 1L0 (sqrt 2L0)) (/ 1L0 4L0) 1L0)
