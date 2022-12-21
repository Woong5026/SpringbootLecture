이전에는 잘 되다가 코드 수정없던 프로젝트를 오랜만에 열었을때 어노테이션이 전부 빨간줄이 뜬 적이 몇 번있었다.

갑자기 잘되던 것들이 안되는 때가 있다 <br/>
오랜만에 킨 프로젝트에서는 Lombok 어노테이션이 모두 error가 났다. <br/>
이 외에도 img 등 static파일이 적용이 안되는 상황  등 많은 상황들이 의외로 간단하게 이 해결 방법으로 해결 된다.

### Build에러


원인은 Gradle이 build되어 있지 않았던 상태이며 <br/>
해결법은 간단히 gradle을 build해주는 것으로 해결된다

![image](https://user-images.githubusercontent.com/78454649/208963312-ec65de14-e072-4458-bd1a-b521b09bda94.png)

<br/>

### Cannot resolve symbol

위의 방법 외에도 멀쩡하게 잘 돌아가던 프로젝트에 빨간줄들이 있고
cannot resolve symbol import라 쓰여있다면 



![image](https://user-images.githubusercontent.com/78454649/208963811-905dd341-6618-4e84-81db-8a4994822da3.png)


File> Invalidate Caches/Restart를 해주면 된다 

