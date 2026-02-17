<template>
  <div>
    <div class="pnl-search">
      <table>
        <colgroup>
          <col width="60px;" />
          <col width="15%" />
          <col width="80px;" />
          <col width="15%" />
          <col width="80px;" />
          <col width="15%" />
          <col width="100px;" />
          <col width="15%" />
        </colgroup>
        <tbody>
          <tr style="margin-bottom:10px;">
            <th id="hide4">대학</th>
            <td>
              <select
                :style="{ 'background-image': 'url(/bg-select.png)' }"
                name="p_daehak"
                id="p_daehak"
                style="width: 100%"
              >
                <option value="A01008">문과대학</option>
                <option value="A07340" selected="">소프트웨어융합대학</option>
              </select>
            </td>
            <th id="hide5">학과</th>
            <td>
              <select
                :style="{ 'background-image': 'url(/bg-select.png)' }"
                name="p_major"
                style="width: 100%"
              >
                <option value="A07337">
                  소프트웨어융합대학 소프트웨어융합학과 소프트웨어융합학
                </option>
                <option value="A07308">
                  소프트웨어융합대학 컴퓨터공학과 컴퓨터공학
                </option>
              </select>
            </td>
            <th id="hide6">강좌명</th>
            <td>
              <input   @keydown.space="keyPrevent"
                @keydown.enter="keyPrevent" type="text" placeholder="검색어 (Search Word)" />
            </td>
            <th id="hide10">언어구분</th>
            <td>
              <select
               :style="{ 'background-image': 'url(/bg-select.png)' }"
                name="p_lang"
                id="p_lang"
                style="width: 100%"
              >
                <option value="">---- All ----</option>
                <option value="1">영어</option>
                <option value="2">영어(부분)</option>
                <option value="3">제2외국어</option>
              </select>
            </td>
          </tr>
          <tr>
            <th id="hide7">요일</th>
            <td>
              <select
                :style="{ 'background-image': 'url(/bg-select.png)' }"
                name="p_day"
                style="width: 100%"
              >
                <option value="">- All -</option>
                <option value="2">월요일</option>
                <option value="3">화요일</option>
                <option value="4">수요일</option>
                <option value="5">목요일</option>
                <option value="6">금요일</option>
                <option value="7">토요일</option>
              </select>
            </td>
            <th id="hide8">수업시간</th>
            <td>
              <select
                :style="{ 'background-image': 'url(/bg-select.png)' }"
                name="p_time"
                id="p_time"
                style="width: 100%"
              >
                <option value="">- All -</option>
                <option value="0900">09:00</option>
                <option value="1500">15:00</option>
              </select>
            </td>
            <th id="hide9">교수명</th>
            <td>
              <input 
                @keydown.space="keyPrevent"
                @keydown.enter="keyPrevent"
              type="text" placeholder="검색어 (Search Word)" />
            </td>
            <th id="hide11">학수번호<br />분반</th>
            <td>
              <input
                v-model="courseNum"
                @keydown.space="keyPrevent"
                @keydown.enter="keyPrevent"
                type="text"
                class="courseINumber"
                placeholder="검색어 (Search Word)"
              />
            </td>
          </tr>
        </tbody>
      </table>
      <div class="buttonset">
        <button style="border:none;"
          @click="searchCourse"
          :style="{ 'background-image': 'url(search.png)' }"
          class="btn-search"
        >
          조회
        </button>
      </div>
    </div>

    <div class="search-content">
      <div class="itb-wrapper">
        <div class="itb search-itms">
          <div class="itm">
            <a class="emt"></a>
            <a class="itm-apply">수강신청</a>
            <a class="itm-snum">학수번호-분반</a>
            <a class="itm-cname">강좌명</a>
            <a class="itm-grd">대상학년</a>
            <a class="itm-znum">정원</a>
            <a class="itm-zanum">잔여인원</a>
            <a class="itm-stime">신청가능시간</a>
            <a class="itm-gname">교수명</a>
            <a class="itm-hakzum">학점</a>
            <a class="itm-time">강의시간/강의실</a>
            <a class="itm-isgub">이수구분</a>
            <a class="itm-lang">언어구분</a>
            <a class="itm-special">특이사항</a>
          </div>
          
          <div v-if="loading" class="loading">
            <div class="loading-bar">Loading ...</div>
          </div>
          
          <div v-for="(course, index) in contents" :key="course.snum" class="itm">
            <a class="emt">{{ index + 1 }}</a>
            <a class="itm-apply">
              <button @click="courseApply(course.snum)" class="apply-btn">신청</button>
            </a>
            <a class="itm-snum">{{ course.snum }}</a>
            <a style="text-align:left;" class="itm-cname">{{ course.cname }}</a>
            <a class="itm-grd">{{ course.grd }}</a>
            <a class="itm-znum">{{ course.znum }}</a>
            <a class="itm-zanum">{{ course.zanum }}</a>
            <a class="itm-stime">{{ course.stime }}</a>
            <a class="itm-gname">{{ course.gname }}</a>
            <a class="itm-hakzum">{{ course.hakzum }}</a>
            <a class="itm-time" style="white-space: pre-line; line-height: 1.3;">{{ course.time }}</a>
            <a class="itm-isgub">{{ course.isgub }}</a>
            <a class="itm-lang">{{ course.lang }}</a>
            <a class="itm-special">{{ course.special }}</a>
          </div>
          
        </div>
      </div>
      <div class="itm-notice">
        <a><i style="margin: 0 5px 0 15px; color: #39abd4; font-size: 20px" class="fas fa-exclamation-circle"></i>
          이수구분 : 11 전공기초, 04 전공필수, 05 전공선택, 06 교직과, 14 중핵교과, 15 배분이수교과, 16 기초교과, 17 자유이수, 20 교직전선, 24 계절학기전공필수, 27 계절학기전공기초, 08 자유선택교과[배움학점제,군사학,취업스쿨,학점교류 과목등]
        </a>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  props: ["isSearch"],
  data() {
    return {
      loading: true,
      contents: [],
      courseNum: ''
    };
  },
  created() {
    setTimeout(() => {
      this.loading = false;
      // 화면이 처음 켜질 때 전체 강의 목록을 불러옵니다.
      this.contents = this.getAllCourses();
    }, 300);
  },
  methods: {
    // 강의 목록 데이터를 따로 분리하여 관리하기 쉽게 만들었습니다.
    getAllCourses() {
      return [
        // 이전에 추가했던 2개 과목
        { snum: "SWCON401-00", cname: "소프트웨어융합캡스톤디자인", grd: "4", znum: 5, zanum: 3, stime: "", gname: "이성원", hakzum: "3.0", time: "금 15:00-17:45 (글404)", isgub: "04", lang: "", special: "" },
        { snum: "EE211-03", cname: "확률및랜덤변수(소프트웨어융합학)", grd: "2", znum: 60, zanum: 30, stime: "", gname: "송주빈", hakzum: "3.0", time: "화 15:00-16:15 (전220),\n목 15:00-16:15 (전220)", isgub: "11", lang: "", special: "" },
        
        // 새로 올려주신 과목들
        { snum: "SWCON104-02", cname: "웹/파이선프로그래밍", grd: "1", znum: 100, zanum: 50, stime: "", gname: "이성원", hakzum: "3.0", time: "- (온라인)", isgub: "04", lang: "영어(부분)", special: "" },
        { snum: "SWCON103-00", cname: "디자인적사고", grd: "1", znum: 25, zanum: 13, stime: "", gname: "이승규", hakzum: "3.0", time: "금 09:00-12:50 (B09)", isgub: "04", lang: "", special: "" },
        { snum: "SWCON104-00", cname: "웹/파이선프로그래밍", grd: "1", znum: 150, zanum: 75, stime: "", gname: "박상근", hakzum: "3.0", time: "화 13:00-14:50 (전205),\n목 13:00-14:50 (전205)", isgub: "04", lang: "", special: "" },
        { snum: "CSE103-01", cname: "객체지향프로그래밍(소프트웨어융합학)", grd: "1", znum: 100, zanum: 50, stime: "", gname: "이대호", hakzum: "3.0", time: "- (온라인)", isgub: "04", lang: "", special: "" },
        { snum: "CSE201-00", cname: "이산구조(소프트웨어융합학)", grd: "1", znum: 70, zanum: 35, stime: "", gname: "박철준", hakzum: "3.0", time: "화 12:00-13:15 (B01),\n목 12:00-13:15 (B01)", isgub: "05", lang: "", special: "" },
        { snum: "SWCON103-02", cname: "디자인적사고", grd: "1", znum: 25, zanum: 13, stime: "", gname: "박광훈", hakzum: "3.0", time: "수 09:00-12:50 (전137)", isgub: "04", lang: "", special: "" },
        { snum: "SWCON104-01", cname: "웹/파이선프로그래밍", grd: "1", znum: 150, zanum: 75, stime: "", gname: "조명아", hakzum: "3.0", time: "화 10:30-12:20 (전205),\n목 10:30-12:20 (전205)", isgub: "04", lang: "", special: "" },
        { snum: "SWCON103-01", cname: "디자인적사고", grd: "1", znum: 25, zanum: 13, stime: "", gname: "조명아", hakzum: "3.0", time: "화 15:00-18:50 (전103)", isgub: "04", lang: "", special: "" },
        { snum: "CSE201-01", cname: "이산구조(소프트웨어융합학)", grd: "1", znum: 70, zanum: 35, stime: "", gname: "최경식", hakzum: "3.0", time: "월 10:30-11:45 (B01),\n수 10:30-11:45 (B01)", isgub: "05", lang: "", special: "" },
        { snum: "AMTH1004-10", cname: "선형대수", grd: "1", znum: 50, zanum: 25, stime: "", gname: "박지연", hakzum: "3.0", time: "금 09:00-11:45 (전102)", isgub: "11", lang: "", special: "" },
        { snum: "AMTH1004-11", cname: "선형대수", grd: "1", znum: 50, zanum: 25, stime: "", gname: "박지연", hakzum: "3.0", time: "금 13:30-16:15 (전102)", isgub: "11", lang: "", special: "" },
        { snum: "AMTH1009-15", cname: "미분적분학", grd: "1", znum: 50, zanum: 25, stime: "", gname: "차준심", hakzum: "3.0", time: "금 09:00-11:45 (전101)", isgub: "11", lang: "", special: "" },
        { snum: "AMTH1009-16", cname: "미분적분학", grd: "1", znum: 50, zanum: 25, stime: "", gname: "차준심", hakzum: "3.0", time: "금 13:30-16:15 (전101)", isgub: "11", lang: "", special: "" },
        { snum: "APHY1002-14", cname: "물리학및실험1", grd: "1", znum: 25, zanum: 13, stime: "", gname: "이호선", hakzum: "3.0", time: "화 09:00-10:50 (멀302),\n목 09:00-10:50 (멀403)", isgub: "11", lang: "", special: "실험수업 302호" },
        { snum: "APHY1002-00", cname: "물리학및실험1", grd: "1", znum: 25, zanum: 13, stime: "", gname: "임대영", hakzum: "3.0", time: "월 11:00-12:50 (멀403),\n수 11:00-12:50 (멀302)", isgub: "11", lang: "", special: "실험수업 302호" },
        { snum: "GEC1105-G40", cname: "성찰과표현", grd: "", znum: 30, zanum: 15, stime: "", gname: "조은영", hakzum: "3.0", time: "화 15:00-16:15 (멀207),\n목 15:00-16:15 (멀207)", isgub: "16", lang: "", special: "" },
        { snum: "GEC1102-G16", cname: "인간의가치탐색", grd: "", znum: 25, zanum: 13, stime: "", gname: "김병진", hakzum: "3.0", time: "월 13:30-14:45 (멀401),\n수 13:30-14:45 (멀401)", isgub: "14", lang: "", special: "" },
        { snum: "GEE1952-G04", cname: "전공탐색및기업가정신세미나", grd: "", znum: 70, zanum: 35, stime: "", gname: "황효석", hakzum: "1.0", time: "화 18:00-18:50 (B09)", isgub: "17", lang: "", special: "" }
      ];
    },
    
    searchCourse() {
      this.contents = []; 
      this.loading = true;
      
      setTimeout(() => {
        this.loading = false; 
        
        // 1. 먼저 전체 강의 목록을 불러옵니다.
        const allCourses = this.getAllCourses();

        // 2. 검색어(courseNum)가 입력되었는지 확인합니다.
        if (this.courseNum.trim() !== "") {
          // 검색어가 있다면, 학수번호(snum)에 검색어가 포함된 강의만 걸러냅니다(필터링).
          // 대소문자 상관없이 검색되도록 모두 소문자로 바꿔서 비교합니다.
          this.contents = allCourses.filter(course => 
            course.snum.toLowerCase().includes(this.courseNum.trim().toLowerCase())
          );
        } else {
          // 검색어를 아무것도 입력하지 않고 조회하면 전체 강의를 보여줍니다.
          this.contents = allCourses;
        }

        // 3. 필터링된 결과의 개수를 부모 컴포넌트로 전달하여 '조회건수'를 업데이트합니다.
        this.$emit('searchNum', this.contents.length);
        
      }, 300); // 0.3초 뒤에 결과 표시 (실제 서버 통신처럼 지연 효과)
    },
    
    keyPrevent(e) {
      alert('매크로 방지를 위해 엔터키와 스페이스바 키는 사용하실 수 없습니다.');
      e.returnValue = false;
    },
    
    courseApply(snum) {
      setTimeout(() => {
        this.$emit('apply', snum); 
      }, 1000);
    }
  },
};
</script>

<style>
/* 강좌 목록 행의 높이를 내용에 맞게 자동 조절 */
.search-itms .itm {
  height: auto !important; /* 기존에 고정된 height 무시 */
  min-height: 40px; /* 너무 좁아지지 않게 최소 높이 지정 */
  padding: 8px 0; /* 위아래 여백을 줘서 답답하지 않게 조절 */
}

/* 글자 줄바꿈 시 깔끔하게 처리 */
.search-itms .itm a {
  white-space: normal !important; /* 강제 한줄 고정(nowrap) 해제 */
  word-break: keep-all; /* 단어가 중간에 잘리지 않게 줄바꿈 */
  line-height: 1.3; /* 두 줄 이상일 때 글자 위아래 간격 확보 */
}
.pnl-search {
    margin-top: 15px;
    position: relative;
    margin-bottom: 15px;
    padding: 11px 20px;
    border-radius: 6px;
    border: 1px solid #e5e5e5;
}
.pnl-search table {
    width: calc(100% - 100px);
    max-width: 1300px;
    table-layout: fixed;
    border-spacing: 0;
    position: static;
    display: table;
    border-collapse: separate;
    text-indent: initial;
    border-color: grey;
}
.pnl-search td+th {
    padding-left: 25px;
}
.pnl-search th {
    padding: 0 0 2px;
    font-size: 12px;
    line-height: 1.1;
    word-spacing: -.5px;
    text-align: left;
    vertical-align: middle;
}
.pnl-search tbody {
    display: table-row-group;
    vertical-align: middle;
    border-color: inherit;
}
.pnl-search tr {
    display: table-row;
    vertical-align: inherit;
    border-color: inherit;
}
.pnl-search table th {
    position: relative;
    padding: 7px 3px 0 0;
    font-weight: 700;
    color: #4d4d4d;
    text-align: right;
    vertical-align: top;
}
.pnl-search table td {
    padding: 0 0 5px 5px;
}
.pnl-search select {
    outline: none;
    appearance: none;
    width: 100%;
    height: 34px;
    font-weight: 400;
    font-size: 13px;
    line-height: 34px;
    letter-spacing: -.5px;
    border: 1px solid #b9b9b9;
    border-radius: 0;
    box-sizing: border-box;
    border-color: #ccc;
    padding: 0 26px 2px 5px;
    background-repeat: no-repeat;
    background-position: right 8px top 50%;
    line-height: 28px;
}
.pnl-search input{
    width: 100%;
    height: 34px;
    font-weight: 400;
    font-size: 13px;
    line-height: 34px;
    letter-spacing: -.5px;
    border: 1px solid #b9b9b9;
    border-radius: 0;
    -moz-appearance: none;
    appearance: none;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    border-color: #ccc;
    outline: none;
    padding: 0 5px;
}
.pnl-search button.btn-search {
    position:absolute;
    top: 12px;
    right: 20px;
    background-color: #444;
    border-color: #444;
    color: #fff;
    background-repeat: no-repeat;
    background-position: 12px 8px;
    padding-left: 32px;
    box-sizing: border-box;
    width: 70px;
    height: 34px;
    line-height: 34px;
    font-size: 13px;
    font-weight: 400;
    cursor: pointer;
    border-radius: 3px;
}
.loading {
  width: 100%;
  height: 400px;
  background-color: white;
  position: relative;
}
.loading .loading-bar {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%);
  width: 83px;
  height: 36px;
  line-height: 36px;
  text-align: center;
  background-color: #444;
  color: #fff;
  font-weight: 400;
  font-size: 14px;
  border-radius: 3px;
}

</style>