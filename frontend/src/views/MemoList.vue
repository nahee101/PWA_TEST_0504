<template>
    <div>
        <h1>메모</h1>
        <!--  메모의 아이디와 작성자와 제목 출력 -->
        <table>
            <tr>
                <td>아이디</td>
                <td>작성자</td>
                <td>제목</td>
            </tr>
            <tr v-for="memo in memo" :key="memo.i">
                <td>{{memo.id}}</td>
                <td>{{memo.writer}}</td>
                <!-- 타이틀을 눌렀을때 /list/:id 링크로 이동 -->
                <td @click="$router.push(`/list/${memo.id}`)">{{memo.title}}</td>
            
                <!--수정&삭제 버튼-->
                <td><button @click="$router.push(`/updateform/${memo.id}`)">수정</button></td>
                <td><button @click="deletememo(memo.id)">삭제</button></td>
            </tr>
        </table>
    </div>
</template>

<script>
export default {
    data () {
        return {
            //v-for로 확인하기 위해 배열
            memo : [ {
                id :1,
                title : "첫번째 메모입니다",
                writer : "green"
            }]
        }
    },
    // 컴포지션이 처음 생성될때 값을 가져와야 화면에 출력할수 있다
    created() {
        // axios를 통해 값을 가져옴 - get
        // this.memo에 가져온 값 넣어줌
        this.$http.get('/api/memo')
        .then((response) => {
            // data를 통해서 값 가져올 수 있다.
            console.log(response.data);
            this.memo = response.data
        })
    },
    methods: {
        deletememo(id) {
            //axios를 통해 id 값을 가진 memo를 삭제한다.
            this.$http.delete(`/api/memo/${id}`)
            .then((response) => {
                // 값이 삭제된 메모 배열을 다시 할당한다
                // 서버로 요청하여 값이 삭제되었지만
                // 화면에 적용하기 위하여 메모 배열을 들고온다
                this.memo = response.data;
            })
        }
    }
}
</script>