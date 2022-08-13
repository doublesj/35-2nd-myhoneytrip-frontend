# PROJECT : myhoneytrip🐝

### • 프로젝트 소개
>**마이리얼트립을 모티브로 한 항공권 예매사이트**  
마이허니트립은 마이리얼트립을 모티브로 허니문 항공권 예약과 관련된 사용자의 소셜 로그인(kakao)과 검색결과 기반 상세페이지, 항공권 예약, 마이페이지까지 이어지는 커머스 사이트의 기본적인 Flow 기능을 구현한 사이트입니다.

### • 작업기간 : 8월 1일 ~ 8월 12일 (12일)

### • 팀원 소개
> wecode 35기 2차 프로젝트 마이허니트립 TEAM

  **FE** | 구단희, 김익현, 신수정, 이강철(PM)  
  **BE** | 황유정, 음정민, 안상현
  
### • 사용기술 및 협업 도구  
> FE: Javascript, React, React-Router, styled-components  
>     library : slick slide, react date picker, mui modal, mul table  
> BE: Phython, Django, MySQL, MINICONDA3, POSTMAN  
> Common : Git&Github, AWS 
> Comunication : Notion, Slack, Trello, Github 

### • 구현기능 및 사용 기술 소개 

```
구단희 : 로그인 페이지, 메인 배너 Carousel, Nav, Footer
김익현 : 로딩 페이지, 항공편 예약 및 마이페이지(예약 확인 및 취소)
신수정 : 검색 상품 리스트 페이지, 검색 결과 filtering
이강철 : 메인 검색창 (캘린더 라이브러리), 최근 검색 항공편 배너 노출
```

### • Site DEMO

https://user-images.githubusercontent.com/99232122/184281715-92bcc9a4-fe11-4405-9c61-a79ed58b75f0.mov  

<http://2nd-myhoneytrip.s3-website.ap-northeast-2.amazonaws.com/>

#### 1. 카카오 로그인 API
- useEffect를 활용한 인가코드 발급 후 서버 전달 (FE 구단희)
#### 2. 메인 페이지
- Slick library를 활용한 Carousel (FE 구단희)
- Search Bar 클릭 시 모달 및 달력 모달창 library 구현 (FE 이강철)
- 최근 검색 결과 상위 배너로 노출 (FE 이강철)
- mock data로 호출한 추천상품 노출 (FE 이강철)

####  3. 검색 상품 리스트 페이지
- 데이터를 불러오기 전 로딩페이지 구현 (FE 김익현)
##### - useLocation을 활용해 querystring을 받아와 서버 데이터 요청 (FE 신수정)
```
  const location = useLocation();
  const queryString = location.search;
  const loadingData = location.state;

  useEffect(() => {
    setTimeout(() => {
      fetch(`http://35.90.169.104:8000/flights${queryString}`)
        .then(res => res.json())
        .then(data => setFlightData(data));
    }, 3000);
  }, [queryString]);
```

##### - selectbox를 통한 filter 기능 구현 (feat. searchParams) (FE 신수정)
```
const CheckBox = ({ queryString, setFlightData }) => {
  const [isChecked, setIsChecked] = useState({
    HoneyAirline: true,
    MoonAirline: true,
  });

  const [searchParams, setSearchParams] = useSearchParams();

  const checkboxFetch = useCallback(() => {
    if (isChecked.HoneyAirline === false && isChecked.MoonAirline === false) {
      searchParams.delete('airline');
      searchParams.append('airline', null);
      setSearchParams(searchParams);
    } else if (
      isChecked.HoneyAirline === true &&
      isChecked.MoonAirline === false
    ) {
      searchParams.delete('airline');
      searchParams.append('airline', 'HoneyAirline');
      setSearchParams(searchParams);
    } else if (
      isChecked.HoneyAirline === false &&
      isChecked.MoonAirline === true
    ) {
      searchParams.delete('airline');
      searchParams.append('airline', 'MoonAirline');
      setSearchParams(searchParams);
    } else {
      searchParams.delete('airline');
      searchParams.append('airline', 'MoonAirline');
      searchParams.append('airline', 'HoneyAirline');
      setSearchParams(searchParams);
    }
  }, [
    isChecked.HoneyAirline,
    isChecked.MoonAirline,
    searchParams,
    setSearchParams,
  ]);
  
  ...
  
    useEffect(() => {
    checkboxFetch();
  }, [checkboxFetch, isChecked]);

  const handleIsChecked = e => {
    const { name, checked } = e.target;
    setIsChecked({ ...isChecked, [name]: checked });
  };
  
```
##### - useNavtigate안에 데이터를 담아 다음 페이지로 전달 (FE 신수정)
```
<S.FlightCostBox
           onClick={() => {
           const newSendingData = [...sendingData];
           departure_airport_code === 'SEL'
          ? (newSendingData[0] = ticket)
          : (newSendingData[1] = ticket);
          setSendingData([...newSendingData]);
          departure_airport_code !== 'SEL' &&
          goToPurchase(newSendingData);
       }}
    >
```   
#### 4. 구매 상품 확인 페이지 (FE 김익현)
- useLocation을 활용해 이전 페이지에서 보내준 데이터 시각화 
- useNavtigate안에 데이터를 담아 다음 페이지로 전달

#### 5. 탑승객 및 예약자 정보 입력 페이지 (FE 김익현)
- 가상의 배열을 만들어 탑승객 수만큼 입력 페이지 생성
- 서버로 전달하기 위한 데이터 가공

#### 6. 마이 페이지 - 예약확인 및 예약취소 (FE 김익현)
- 현재 탭에 따른 데이터 요청 예약취소 시 patch를 사용해 데이터 상태 변경  

#### 7. NAV (FE 구단희)
- 로그인/로그아웃 상태변경 및 마이페이지 이동 구현
 
#### 8. Footer (FE 구단희)
- 상수데이터 활용하여 구성

### • 참고
#####
이 프로젝트는 마이리얼트립 사이트를 참조하여 학습 목적으로 만들었습니다.  
실무수준의 프로젝트이지만 학습용으로 만들었기 때문에 이 코드를 활용하여 이득을 취하거나 무단 배포할 경우 법적으로 문제될 수 있습니다.  
이 프로젝트에 사용된 모든 영상 및 이미지, 폰트 정보는 저작자 표기가 필요하지 않는 Royalty-free를 사용했습니다.  
