# Animate the modal to smoothly turn on

- react 컴포넌트 모달창을 띄울 때 styledcomponents의 keyframse를 사용해서 모달이 부드럽게 켜지는 효과를 줄 수 있다.

- @keyframes @규칙은 개발자가 애니메이션 중간중간의 특정 지점들을 거칠 수 있는 키프레임들을 설정함으로써 CSS 애니메이션 과정의 중간 절차를 제어할 수 있게 함.

  이 룰은 브라우저가 자동으로 애니메이션을 처리하는 것 보다 더 세밀하게 중간 동작들을 제어할 수 있다.

  ```js
  // 배경 opacity animation
  const modalBgShow = keyframes`
    from {
      opacity : 0;
    }
    to {
      opacity : 0.8;
    }
  `;

  // 모달창 opacity animation과 위에서 내려오는 효과를 준다.
  const modalShow = keyframes`
    from {
      opacity : 0;
      margin-top :-50px;
    }
    to {
      opacity : 1;
      margin-top : 0;
    }
  `;
  ```

  ```js
  const ModalOverlay = styled.div`
    ...

    animation: ${modalBgShow} 0.5s;
    // 모달이 켜질 때 배경 opacity를 animation으로 조절해준다.
  `;

  const ModalContainer = styled.div`
    ...

    animation: ${modalShow} 0.5s;
    // 모달이 켜질 때 모달 창 opacity를 animation으로 조절해준다,
  ```
