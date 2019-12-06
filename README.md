# dynamic-vue-select
사용배경. 회사에서 개발중.. bootstrap 기반 콤보박스 ui 컴포넌트(vue-select)를 사용중 페이징 기능이 없는거 같아..
문의를 남겨보아도 별 응답이 없어(영어를 잘못쓴건지??) 해서 기존 컴포넌트를 확장해서 만든 컴포넌트

# Before Use
1. vue-select 라이브러리 다운
``` npm install vue-select ```
2. 페이징 처리(각 페이지에 맞는 아이템 갯수와 총 아이템 갯수를 보내주는) 가 되는 API

# Usage

컴포넌트 사용 예시
```
  <dynamic-vue-select v-model="element"
                      @search:focus="getListToServer"
                      :options="list"
                      :per-element="5"
                      placeholder="input custom placeholder"
                      label="name">
  </dynamic-vue-select>
```

v-model은 선택된 Object,
@search:focus는 최초 클릭스 실행할 함수 (예: 서버에서 리스트 가져오는함수)
:options는 셀렉트 태그에 보여질 옵션 목록들
:per-element는 한번에 가져올 리스트 갯수
label은 ui에 보여질 v-model의 Object key

리스트 가져오는 함수 예시
```
async getListToServer (count = 1, perElement = 5) {
  try {
    // 서버에서 리스트와 총 리스트 갯수를 준다고 가정
    const { data: { list, totalCount } } = await api.getList(count, perElement)
    // 총 갯수보다 현재 리스트의 갯수가 작으면 더보기를 임의로 생성해서 넣어줌.. 
    // moreAction은 dynamic-vue-select에서 사용하는 property기 때문에 key값은 통일
    // name property는 상단 label property와 동일한것을 사용
    // onClick도 역시 dynamic-vue-select에서 사용한다.
    if (this.list.length < totalCount) {
      const obj = {
        name: '더보기...',
        moreAction: true,
        onClick: (count, perElem) => this.getListToServer(count, perElem)
      }
      // 리스트 맨 하단에 더보기... option을 추가한다.
      this.list.push(obj)
    }
  } catch (error) {
    // 각자 알아서 에러처리..
    errorHandler(error)
  }
  
}
```
