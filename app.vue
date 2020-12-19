new Vue({
    el: "#app",
    data : {
        game_is_on   : true,
        player_healt : 100,
        monster_healt : 100,
        specialCompleted : true,
        healCompleted : true,
        logs : [

        ]
    },
    methods : {
        start_game : function(){
            this.game_is_on = !this.game_is_on;
            this.add_to_log({ turn : "p" , text : "Oyun Başladı"})
        },
        attack : function(){    
            var point = Math.ceil(Math.random()*10);
            this.monster_healt -= point;
            this.add_to_log({ turn : "p" , text : "Oyuncu Atağı ("+ point +")"})
            this.monsterAttack();
            this.info()
        },
        specialAttack : function(){
            var point = Math.ceil(Math.random()*30);
            this.monster_healt -= point;
            this.add_to_log({ turn : "p" , text : "Özel Oyuncu Atağı ("+ point +")"})
            this.monsterAttack();
            this.info();
            this.specialCompleted = false;
        },
        heal : function(){
            this.player_healt += 15;
            this.info();
            this.healCompleted = false;
            this.add_to_log({ turn : "p" , text : "Oyuncu Ilk Yardım (15)"})
            this.monsterAttack();     
        },
        giveUp : function(){
            this.player_healt = 0;
            this.add_to_log({ turn : "p" , text : "Pes edildi"})
        },
        monsterAttack : function(){
            var monsterPoint = Math.ceil(Math.random()*15);
            this.player_healt -= monsterPoint;
            this.add_to_log({ turn : "m" , text : "Canavar Atağı ("+ monsterPoint +")"})
        },
        info : function(){
            console.log(`
                Player : ${this.player_healt}
                Monster : ${this.monster_healt}
            `)
        },
        add_to_log : function(log){
            this.logs.push(log);    
        }
    },

    watch : {
        player_healt : function(value){
            if(value <= 0){
                this.player_healt = 0;
                if(confirm("Oyunu kaybettin. Tekrar ister misin ?")){
                    this.player_healt = 100;
                    this.monster_healt = 100;
                    this.specialCompleted = true;
                    this.healCompleted = true;
                    this.logs = []
                }
            }else if( value >=100){
                this.player_healt = 100;
            }
        },
        monster_healt : function(value){
            if(value <= 0){
                this.monster_healt = 0;
                if(confirm("Oyunu kazandın. Tekrar ister misin ?")){
                    this.player_healt = 100;
                    this.monster_healt = 100;
                    this.specialCompleted = true;
                    this.healCompleted = true;
                    this.logs = [];
                }
            }
        }
    }
});