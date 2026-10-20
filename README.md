#VisionPredator-InHouse

G-STAR 2024 전시 작품
연구소의 실험체인 플레이어는 총기와 특수 능력을 활용하여 적을 처치하고 생존해야 합니다.

| 항목 | 내용 |
| --- | --- |
| **프로젝트명** | Vision Predator |
| **플랫폼** | PC |
| **개발 언어** | C++, HLSL |
| **사용 기술** | DirectX 11 |
| **담당 작업** |  렌더링 엔진 설계, Deferred Rendering 구현, Screen Space Decal 및 PBR 구현을 통한 렌더링 품질 상승 |

자세한 프로젝트 정보는 [여기](https://github.com/VisionPredator)에서 확인 가능합니다.


이 리포지토리는 VisionPredator 프로젝트에서 기여한 그래픽스 작업을 정리 해놓은 리포지토리입니다.


## 구현 상세
### 1. [RenderPass 기반 렌더링 파이프 라인 설계](./GraphicsEngine/Renderer/RenderPass.h)

<table>
  <tr>
    <td align="center">
      <br />
      <img width="426" height="240" alt="Video Project 8" src="https://github.com/user-attachments/assets/8c88d544-1a6e-4ad6-9873-0fa4a13e4265" />
      <sub><b> 실시간 RenderPass 변경</b></sub>
      </td>
    <td align="center">
    <img width="426" height="240" alt="Video Project 7 (1)" src="https://github.com/user-attachments/assets/c89b306a-5a37-4d08-bffb-1cdbfb617e2f" />
    <br />
      <sub><b> 인게임 (VP)특수능력 피드백으로 활용 </b></sub>
    </td>
  </tr>
</table>

하나의 RenderData를 여러 RenderPass에서 재사용하도록 구조를 활용하여, 기존 Model Class 중심의 구조에서 발생하는 중복 데이터로 인한 메모리 사용 증가와 관리 비용 문제를 개선했습니다.

<br>

<p align="center">
<img width="605" height="338" alt="image" src="https://github.com/user-attachments/assets/ee5dca56-93b2-448b-a272-ef9e7e981e7c" />


| 항목 | 내용 |
| --- | --- |
| [RenderPass](./GraphicsEngine/Renderer/RenderPass.h) | 오브젝트를 그리는 방법을 정의한 클래스, <br />[DeferredPass](/GraphicsEngine/Renderer/DeferredPass.cpp), [DebugPass](/GraphicsEngine/Renderer/DebugPass.cpp) 등 다양한 파생 RenderPass를 만들어 각기 사용되는 고유의 Shader 및 렌더링 방식을 정의.   |
| [PassManager](/GraphicsEngine/Renderer/PassManager.h) | 여러 RenderPass를 의도한 순서에 동작하도록 하고 일괄적으로 제어 및 관리 담당 |

<br>

### 2. [리소스 관리](./GraphicsEngine/Renderer/ResourceManager.h)

<p align="center">
<img width="771" height="466" alt="image" src="https://github.com/user-attachments/assets/9b7a6ff1-515f-4f95-8c96-c69e6f35ff56" />
<sub><b>프로젝트에서 팀원이 사용한 리소스 생성 코드 - 타입이 다양해도 Create<T>(params…)로 생성</b></sub>
  
<br>
  
<br>
렌더링 과정에서 사용되는 다양한 리소스의 각기 다른 사용법과 관리방식을 동일한 인터페이스로 관리하여,
<br> 반복되는 리소스 사용 작업의 효율성을 높이고자 했습니다.

| 항목 | 내용 |
| --- | --- |
| [Resource](./GraphicsEngine/Renderer/Resource.h) | 렌더링되는 과정에서 사용되는 RT,SRV 등 다양한 타입의 리소스를 일괄 제어하기 위한 기반클래스, <br />[VertexBuffer](/GraphicsEngine/Renderer/VertexBuffer.cpp#L9C1-L17C2), [RenderTargetView](/GraphicsEngine/Renderer/RenderTargetView.cpp#L21C1-L27C2) 등 파생된 클래스들은 생성자에서 고유의 리소스 생성 동작 |
| [ResourceManager](ResourceManager.h#L78C1-L94C2) | 여러 Resource를 일괄적으로 제어 및 관리하는 클래스 |


<br>

### 3.[Screen Space Decal](https://blog.naver.com/gamenlife97/223989962953)

<table>
  <tr>
    <td align="center">
      <br />
      <img width="426" height="240" alt="제목 없는 동영상 - Clipchamp로 제작 (14)" src="https://github.com/user-attachments/assets/c9e0dd4b-2dda-40ee-a520-7fba9fe17f68" />
      <sub><b> 투영평면에 따른 UV좌표 설정 테스트 결과</b></sub>
     </td>
    <td align="center">
    <img width="426" height="240" alt="Video Project 1" src="https://github.com/user-attachments/assets/95077d53-4242-4428-9f6a-d6647d6ee768" />
    <br />
      <sub><b> 인게임 총 발사 피드백으로 활용 </b></sub>
    </td>
  </tr>
</table>

<img width="1202" height="531" alt="image" src="https://github.com/user-attachments/assets/4159dd08-0805-4b8e-8c3a-a29d3321a158" />

<br>

### 4.[Physically Based Rendering](https://blog.naver.com/gamenlife97/223985791792)

<img width="1426" height="746" alt="image" src="https://github.com/user-attachments/assets/6e97129c-3a15-401b-8ff1-e20d55cff45f" />
<sub><b> 에디터 환경에서의 PBR 결과</b></sub>

<br>

### 5.[Image Based Lighting](https://blog.naver.com/gamenlife97/224134354288)
<table>
  <tr>
    <td align="center">
      <br />
<img width="466" height="300" alt="image" src="https://github.com/user-attachments/assets/3eee9839-c7c4-4bdd-856c-9038d12da5ad" />
     </td>
    <td align="center">
<img width="466" height="238" alt="image" src="https://github.com/user-attachments/assets/75ffb064-ff6e-4d5d-87fa-b75d6e0a51c8" />
    <br />
    </td>
  </tr>
</table>


