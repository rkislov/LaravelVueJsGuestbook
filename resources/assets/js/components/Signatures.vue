<template>
    <div>
        <div class="panel panel-default" v-for="signature in signatures">
            <div class="panel-heading">
                <span class="glyphicon glyphicon-user" id="star"></span>
                <label id="started">От</label> {{signature.name}}
            </div>
            <div class="panel-body">
                <div class="col-md-2">
                    <div class="thumbnail">
                        <img :src="signature.avatar" :alt="signature.nama"/>
                    </div>
                </div>
                <p>{{signature.body}}</p>
            </div>
                <div class="panel-footer">
                    <span class="glyphicon glyphicon-calendar" id="visit"></span> {{signature.date}}
                    <span class="glypicon glyphicon-flag" id="comment"></span>
                    <a href="#" id="comments" @click="report(signature.id)">Отметить</a>
                </div>
            </div>
            <paginate
                :page-count="PageCount"
                :click-handler="fetch"
                :prev-text="'Назад'"
                :next-text="'Вперед'"
                :container-class="'pagination'">
            </paginate>
        </div>

</template>
<script>
    export default {
    data(){
        return {
            signatures: [],
            pageCount: 1,
            endpoint: 'api/signatures?page='
        };
    },
        created() {
        this.fetch();
        },

        methods: {
        fetch(page = 1){
            axios.get(this.endpoint+page)
                .then(({data}) => {
                    this.signatures = data.data;
                    this.pageCount = data.meta.last_page;
            });
        },
            report(id){
            if(confirm('Вы уверенны, что хотите отправить этот отзыв')) {
                axios.put('api/signatires'+id+'/report')
                .then(response => this.removeSignature(id));
            }
            },

            removeSignature(id){
            this.signatures = _.remove(this.signatures, function (signature) {
                return signature.id !== id;
            });
            }
        }
    }
</script>