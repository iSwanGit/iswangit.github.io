---
layout: post 
# title: 
# subtitle:
category: technical
# tags: [typescript, algorithm]
description: >
  내가 필요해서 만든 스택 ES6 구현체
# image: ''
related_posts: ''
random_posts: ''
---

# ES6 (TypeScript) 환경에서 Stack 사용하기

Node.js로 할 수 있는 것들, ECMAScript 버전업 등 역사적인 이야기는 둘째치고, TypeScript 등장으로 JavaScript 생태계에 쉽게 입문할 수 있었다. 특히 작년 Angular 프레임워크를 처음 만져보게 되면서 TS를 써보고, class를 사용할 수 있다는 것이 굉장히 매력적으로 느껴졌던 기억이 있다. 지금도 node 기반의 프로그램을 만들 때 typescript를 반드시 추가하고 프레임워크가 아니어도 class를 이용한 객체지향 설계를 추구하는 편이다. (ts 없으면 답답해서 어떻게 쓴대;)

아무튼, ECMAScript 6에서는 크게 Array, Map, Set 기반의 컬렉션이 존재한다. 하지만 개발하다 보니 LIFO를 따르는 자료구조가 필요한데 딱맞는 자료구조가 없다. 어차피 배열 형식으로 써도 push와 pop이 구현되어 있으니 큰 문제는 없으나, 개인적으로 그다지 보기 좋지 않아서 별 대단한 건 아니지만 Stack 타입을 새로 만들어보았다.

## 스택(Stack)
- 후입선출 (LIFO: Last In First Out)
- push, pop, peek(top), clear, isEmpty
- 복사 생성자

```ts
/**
 * Stack data structure using generic type
 * @license MIT
 * @copyright 2021 iSwan (EleMas*) <iswan@iswan.kr>
 * @class
 */
export class Stack<T> {
  private arr: T[];
  public get size(): number {
    return this.arr.length;
  }

  /**
   *  Stack 생성
   *  @param stack 복사할 스택 (optional)
   */
  constructor(stack?: Stack<T>) {
    if (stack) {
      this.arr = stack.arr.map(v => v); // deep copy
    }
    else {
      this.arr = [];
    }
  }

  /**
   * 스택에 요소 추가
   * @param item new item to add
   */
  public push(item: T): void {
    this.arr.push(item);
  }

  /**
   * 스택의 가장 위에 있는 요소 추출 및 제거
   * @returns null if empty
   */
  public pop(): T | undefined {
    if (this.isEmpty()) {
      return undefined;
    }
    else {
      return this.arr.pop();
    }
  }

  /**
   * 스택의 가장 위에 있는 값을 출력
   * @returns null if empty
   */
  public peek(): T | undefined {
    if (this.isEmpty()) {
      return undefined;
    }
    return this.arr[this.arr.length-1];
  }

  /**
   * 스택 비우기
   */
  public clear(): void {
    this.arr.length = 0;
  }

  /**
   * 스택이 비어있는지 확인
   * @returns
   */
  public isEmpty(): boolean {
    return this.arr.length === 0;
  }
}

```

### 여담
"까짓 거" 로 시작한 거라 클래스 만드는 데 시간은 얼마 걸리진 않았지만, 만들어놓고 보니 npm에 stack 자료구조 구현해 둔 패키지가 정말 많이 보인다..ㅋㅋㅋㅋ 특히 [이거](https://github.com/datastructures-js/stack){: target="_blank"}