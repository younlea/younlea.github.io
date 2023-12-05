---
title: "portable llamafile "
excerpt_separator: "<!--more-->"
categories:
  - AI
tags:
  - llamafile

toc : true
toc_sticky : true
---
# [llamafile](https://github.com/Mozilla-Ocho/llamafile)    
이제 llama 설치 등 힘들이지 않고 파일 하나만 실행하고 로컬 호스트에서 써봐요.



## Quickstart

The easiest way to try it for yourself is to download our example 
llamafile for the [LLaVA](https://llava-vl.github.io/) model (license: [LLaMA 2](https://ai.meta.com/resources/models-and-libraries/llama-downloads/), 
[OpenAI](https://openai.com/policies/terms-of-use)). LLaVA is a new LLM that can do more 
than just chat; you can also upload images and ask it questions 
about them. With llamafile, this all happens locally; no data 
ever leaves your computer.

1. Download [llava-v1.5-7b-q4-server.llamafile](https://huggingface.co/jartine/llava-v1.5-7B-GGUF/resolve/main/llava-v1.5-7b-q4-server.llamafile?download=true) (3.97 GB).

2. Open your computer's terminal.

3. If you're using macOS, Linux, or BSD, you'll need to grant permission 
for your computer to execute this new file. (You only need to do this 
once.)

```sh
chmod +x llava-v1.5-7b-q4-server.llamafile
```

4. If you're on Windows, rename the file by adding ".exe" on the end.

5. Run the llamafile. e.g.:

```sh
./llava-v1.5-7b-q4-server.llamafile
```

6. Your browser should open automatically and display a chat interface. 
(If it doesn't, just open your browser and point it at https://localhost:8080.)

7. When you're done chatting, return to your terminal and hit 
```Control-C``` to shut down llamafile.

**Having trouble? See the "Gotchas" section below.**

# 간단 결과
써보니 나름 괜츈한데요 ^^; 

```
User: what's your name?

Llama: My name is Llama!

User: What do you think about meta?

Llama: I think that the concept of "meta" refers to self-awareness, particularly in regards to artificial intelligence or consciousness. It can also refer to a level of abstraction beyond which something becomes difficult to understand or define.

User: could you make python code? I want to solve prime number check code.
input : 1 ~ 100 number
output : prime number in input.

Llama: Sure, here's some Python code that will take an integer between 1 and 100 as input and return a list of all the prime numbers within that range:
def is_prime(n):
    if n < 2 or n == 1:
        return False
    for i in range(2, int(n0.5) + 1):
        if n % i == 0:
            return False
    return True

numbers = list(range(1, 101))
primenumbers = [num for num in numbers if isprime(num)]
print(prime_numbers)

User: could you make using c language?

Llama: Sure! Here's a C program that will take an integer between 1 and 100 as input and return a list of all the prime numbers within that range:
#include <stdio.h>
#include <math.h>

int is_prime(int n);
void get_primes();

int main() {
    int num, i;
    printf("Enter an integer between 1 and 100: ");
    scanf("%d", &num);
    
    get_primes(num);
    
    return 0;
}

void get_primes(int n) {
    int i, j;
    for (i = 2; i <= sqrt(n); i++) { //sqrt is square root function
        if (n % i == 0) {
            printf("%d ", i);
            for (j = i + 1; j * j <= n; j += n / j) {} //if number is not prime then it will be printed
        }
    }
}
```
