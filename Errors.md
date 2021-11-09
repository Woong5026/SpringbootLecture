### could not initialize proxy

요 에러는 말그대로 지연 로딩을 하려는데 이미 세션이 사라져서 지연 로딩을 못할 때 발생하는 에러다.

그래서 보통은 @*ToMany 관계로 되어 있어 기본 FetchType이 Lazy인 멤버를 읽어올 때 발생하며, 그냥 쉬운 해결 법으로는 FetchType.EAGER를 명시해서 즉시 로딩으로 읽어오게 하면 해결될 수도 있다.

하지만, 비즈니스 요구가 아니라 단순히 LazyInitialization 에러를 피하기 위해 즉시 로딩을 선택하는 것은 좋은 해법이 아니다.

요는 지연 조회 시점까지 세션을 유지 시켜주면 되는데 가장 쉽게는 해당 조회가 포함된 메서드에 @Transactional(조회일 때는 @Transactional(readOnly = true))를 붙여주면 된다.

 // 스크린 야구 모임 수정
   ** @Transactional**
    public void updateScreen(Long screenId, ScreenRequestDto requestDto, UserDetailsImpl userDetails){
        String loginedUserId = userDetails.getUser().getUserid();
        String createdUserId = "";

        Screen screen = screenRepository.findByScreenId(screenId);
        if(screen != null) {
            createdUserId = screen.getScreenCreatedUser().getUserid();

            if(!loginedUserId.equals(createdUserId)){
                throw new IllegalArgumentException("수정권한이 없습니다");
            }
            screen.updateScreen(requestDto);
            screenRepository.save(screen);
        }else {
            throw new IllegalArgumentException("해당 게시물이 존재하지 않습니다");
        }

    }

진짜 저 Transactional 하나때문에 몇 시간을 고생했는지 모르겠다..
