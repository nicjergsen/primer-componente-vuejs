<template>
    <div>
        <h2 class="text-center">Captura tus ideas</h2>
        <div class="well">
            <h4>En que estás pensando?</h4>
            <form v-on:submit.prevent="createIdea">
                <div class="input-group">
                    <input type="text" class="form-control" v-model="newIdea" maxlenght="256">
                    <span class="input-group-btn">
                        <button type="submit" class="btn btn-primary">Agregar</button>
                    </span>
                </div>
            </form>
            <hr>
            <ul class="list-unstyled">
                <li v-for="idea in ideas">
                    <p>
                        <small class="text-muted">
                            <em>{{ since(idea.created_at)}}</em>
                        </small>
                        {{ idea.descripcion}}
                    </p>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import toastr from 'toastr'
    import moment from 'moment'

    moment.locale('es');

    export default {
        data (){
            return {
                ideas: [],
                newIdea: '',
                }
            },
            created: function(){
                this.getIdeas();
            },
            methods: {
                since: function(date){
                    return moment(date).fromNow();
                },
                getIdeas: function(){
                    var urlIdeas = 'mis-ideas';
                    axios.get(urlIdeas).then(response => {
                        this.ideas = response.data
                    });
                },
                createIdea: function(){
                    var url = 'guardar-idea';
                    axios.post(url, {
                        descripcion: this.newIdea
                    }).then(response =>{
                        this.getIdeas();
                        this.newIdea = '';
                        toastr.success('Nueva idea registrada');
                    }).catch(error =>{
                         toastr.error('Error');
                    });
                }
            }
        
    }
</script>
