---
title: 'React 컴포넌트 라이프사이클'
date: 2020-01-08
category: 'react'
draft: false
---

![](./images/banner/react.png)

## 개요
- 모든 리액트 컴포넌트에는 라이프사이클(LifeCycle)이 존재한다.
- 컴포넌트의 수명은 페이제에 렌더링되기 전 준비과정에서 페이지가 사라질 때 종료된다.
- **Will** 접두사가 붙은 메서드는 어떤 작업을 작동하기 **전**에 실행되는 메서드고, **Did** 접두사가 붙은 메서드는 어떤 작업을 작동한 후에 실행되는 메서드이다.
- 라이프사이클은 **마운트**, **업데이트**, **언마운트**로 나뉜다.

## 마운트
- DOM이 생성되고 웹 브라우저에 나타나는 것을 마운트라고 한다.

### 마운트 순서 및 간단 설명
1. constructor
    - 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드
2. getDerivedStateFromProps
    - props에 있는 값을 state에 동기화하는 메서드
3. render
    - 준비한 UI를 렌더링하는 메서드
4. componentDidMount
    - 컴포넌트가 웹 브라우저상에 나타난 후 호출하는 메서드.

## 업데이트
- 컴포넌트는 업데이트는 props, state가 바뀌는 경우, 부모 컴포넌트 리렌더링, this.forceUpdate로 강제 렌더링 할때 발생한다.

### 업데이트 순서 및 간단 설명
1. getDeviedStateFromProps
    - props가 바뀌어서 업데이트 할 경우 호출 되는 메서드
2. shouldComponentUpdate
    - 컴포넌트가 리렌더링을 해야 하는지 안해야 하는지 결정 하는 메서드
3. render
    - UI 렌더링 메서드
4. getSnapshotBeforeUpdate
    - 컴포넌트 변화를 DOM에 반영하기 바로 전에 호출 되는 메서드
5. componentDidUpdate
    - 컴포넌트 업데이트 작업 후 호출되는 메서드

## 언마운트
- 마운트된 컴포넌트를 DOM에서 제거하는 것을 언마운트라고 한다.

### 언마운트 순서 및 간단 설명
1. componentWillUnmount
    - 컴포넌트가 웹 브라우저상에서 사라지기 전에 호출 되는 메서드


## 라이프사이클 설명

### render
- 라이프사이클 메서드 중 유일하게 필수 메서드로 안에서 this.props와 this.state에 접근 할 수 있다.
- 요소는 태그나 컴포넌트가 되며 아무것도 표출하지 않을 경우 null이나 false를 반환한다.
- 메서드 안에서 절대 state를 변형하면 안되며 DOM에 정보를 가져오거나 변화를 줄 때는 componentDidMount를 이용한다.

### constructor
- 컴포넌트의 생성자 메서드로 컴포넌트를 만들 때 가장 먼저 실행된다.
- 초기 state를 정한다.

### getDerivedStateFromProps
- 리액트 v16.3 이후 만든 라이프사이클 메서드
- props로 받아 온 값을 state에 동기화 시키는 용도로 사용한다.
- 컴포넌트를 마운트하거나 props를 변경할 경우 호출한다.

### componentDidMount
- 컴포넌트 생성 후 렌더링을 모두 마친 후 호출하는 메서드
- 이벤드 등록, 네트워크 통신 같은 비동기 작업을 처리한다.

### shouldComponentUpdate
- props, state가 변경 되었을 때 리렌더링을 시작할지 여부 를 정하는 메서드
- 메서드를 생성하지 않을경우 default로 true를 반환한다.
- 새로 설정될 props나 state는 nextProps, nextState 파라미터를 통해 접근할 수 있다.

### getSnapshotBeforeUpdate
- 리액트 v16.3 이후 만든 라이프사이클 메서드로 render 메서드를 호출한 후 DOM에 변화를 반영하기 직전에 호출 하는 메서드
- 이 메서드의 반환값은 copmonentDidUpdate에서 세 번째 파라미더인 snapshot 값으로 전달 받아 업데이트 직전에 값을 참고 할 때 사용 된다.

### componentDidUpdate
- 리렌더링을 완료한 후에 실행되는 메서드
- prevProps, prevState를 사용하여 컴포넌트가 이전에 가졌던 데이터에 접근이 가능하다.
- getSnapshotBeforeUpdate 반환 값이 존재하면 snapshot을 통해 값을 전달 받을 수 있다.

### componentWillUnmount
- 컴포넌트가 사라지기 전에 실행되는 메서드
- componentDidMount에서 등록한 이벤트, 타이머, DOM 등 제거 해야 할 일이 있다면 이 메서드에서 제거 작업을 한다.
