<template>
    <section class="seeds-input">
        <p class="seeds-input-title">Добавить новый вид семян</p>
        <div class="seed-name">
            <label for="seed-name">Название нового вида семян</label>
            <input 
                :class="newSeedNameError ? 'input-error' : (newSeedName) ?  'input-valid': ''"
                name="seed-name"
                type="text"
                placeholder="Овес"
                required
                v-model="newSeedName"
            >
            <span v-if="newSeedNameError">
                <span class="seed-name-input-error">
                    {{newSeedNameError}}
                </span>
            </span>
        </div>
        
        <div class="seed-values">
            <p class="seed-values-title">Кол-во, шт./м2 за год</p>
       
            <div class="seed-value">
                <label for="seed-value-2025"> за 2025 год</label>
                <input
                    name="seed-value-2025"
                    type="text"
                    placeholder="35"
                    required
                    minlength="1"
                    maxlength="2"
                    pattern="\d+"
                    v-model="newSeedValue2025"
                >
                <span class="seed-value-input-error">
                   {{newSeedValueError}}
                </span>
            </div>
            
            <div class="seed-value">
                <label for="seed-value-2026"> за 2026 год</label>
                <input 
                    name="seed-value-2026"
                    type="text"
                    placeholder="17"
                    required
                    minlength="1"
                    maxlength="2"
                    pattern="\d+"
                    v-model="newSeedValue2026"
                >
                <span class="seed-value-input-error">
                   {{newSeedValueError}}
                </span>
            </div>
            
            <div class="seed-value">
                <label for="seed-value-2027"> за 2027 год</label>
                <input 
                    name="seed-value-2027"
                    type="text"
                    placeholder="74"
                    required
                    minlength="1"
                    maxlength="2"
                    pattern="\d+"
                    v-model="newSeedValue2027"
                >
                <span class="seed-value-input-error">
                   {{newSeedValueError}}
                </span>
            </div>
            
            <div class="seed-value">
                <label for="seed-value-2028"> за 2028 год</label>
                <input 
                    name="seed-value-2028"
                    type="text"
                    placeholder="9"
                    required
                    minlength="1"
                    maxlength="2"
                    pattern="\d+"
                    v-model="newSeedValue2028"
                >
                <span class="seed-value-input-error">
                   {{newSeedValueError}}
                </span>
            </div>
            
        </div>

        <button 
            @click="sendNewSeedData" 
            :disabled="!(validNewSeedName && validNewSeedValue)">
            Добавить вид семян
        </button>

        <button @click="getRandomSeedData">Случайные данные</button>
    </section>
</template>

<style lang="scss" src="./seedsinput.sass"></style>

<script>
import allSeeds from '../../data/allseeds';

export default {
    props: [
        'currentSeeds'
    ],
    data() {
        return {
            newSeedName: '',
            newSeedValue2025: '',
            newSeedValue2026: '',
            newSeedValue2027: '',
            newSeedValue2028: '',
            validNewSeedName: false,
            validNewSeedValue: false,
            availableSeeds: [],
            namesOfCurrentSeeds: [],
            newSeedNameError: '',
            newSeedValueError: '',
        }
    },

    methods: {

        checkNewSeedName() {
            // проверка названия вида семян

            for(let seed in this.currentSeeds) {
                // currentSeeds - содержит имеющиеся на графике виды семян
                // проверяем, нет ли совпадений с уже выведенными семенами
                if(this.newSeedName.toLowerCase() === this.currentSeeds[seed].name.toLowerCase()) {
                    this.newSeedNameError = 'Данный вид семя уже добавлен на график!';
                    return this.validNewSeedName = false;
                }
            }
            for(let seed in allSeeds) {
                // allSeeds - содержит в себе все известные виды семян
                // проверяем не придумал ли пользователь какой-то новый вид
                if(this.newSeedName.toLowerCase() === allSeeds[seed].toLowerCase()) {
                    this.newSeedNameError = '';
                    return this.validNewSeedName = true;
                }    
            }

            this.newSeedNameError = 'Данный вид семян не существует!';
            return this.validNewSeedName = false;            
        },

        checkNewSeedValue() {
            // проверка входных данных на наличие строки(все значения должны быть числа)
            if( isNaN(+this.newSeedValue2025) || 
                isNaN(+this.newSeedValue2026) ||
                isNaN(+this.newSeedValue2027) ||
                isNaN(+this.newSeedValue2028) ) {
                this.newSeedValueError = 'Поле должно иметь числовой тип!';
                return this.validNewSeedValue = false;
            }
            return this.validNewSeedValue = true;
        },

        getRandomSeedData() {
            // подставляем случайные значения и случайный вид семян(берем из доступных)

            this.currentSeeds.map(el => {
                // запихиваем в namesOfCurrentSeeds имена добавленных на график семян
                this.namesOfCurrentSeeds.push(el.name);
            });

            // availableSeeds содержит массив доступных семян "минус" семена на графике
            this.availableSeeds = allSeeds.filter(el => !this.namesOfCurrentSeeds.includes(el));
            
            if (this.availableSeeds.length === 0) {
                alert('Доступные для добавления виды семян отсутствуют')
            }
            // случайное число, которое будет = одному из 
            // эллементов массива для случайного выбора семян
            let rundNum = Math.floor(Math.random() * this.availableSeeds.length);

            // присваиваем случайное название нового вида семян
            this.newSeedName = this.availableSeeds[rundNum];
            // присваиваем случайные числа от 0 до 60 для значений нового вида семян
            this.newSeedValue2025 = Math.floor(Math.random() * 61);
            this.newSeedValue2026 = Math.floor(Math.random() * 61);
            this.newSeedValue2027 = Math.floor(Math.random() * 61);
            this.newSeedValue2028 = Math.floor(Math.random() * 61);

            this.validNewSeedName = true;
        },

        clearFormData() {
            // осчистка полей формы
            this.newSeedName = ''
            this.newSeedValue2025 = ''
            this.newSeedValue2026 = ''
            this.newSeedValue2027 = ''
            this.newSeedValue2028 = ''
        },

        sendNewSeedData() {
            // проверяем входные данные и отправляем выше
            this.checkNewSeedName();
            this.checkNewSeedValue();
            if(this.validNewSeedName) {
                // Если название валидно, отправляем в родительский компонент
                this.$emit('getNewSeedData', 
                    {
                        name: this.newSeedName,
                        data: [
                            +this.newSeedValue2025, 
                            +this.newSeedValue2026, 
                            +this.newSeedValue2027, 
                            +this.newSeedValue2028
                        ]
                    }, 
                );
                // если данные отправлены успешно, очищаем форму
                this.clearFormData()
            }
        }

    },
    watch: {
        newSeedName: {
            handler() {
                this.checkNewSeedName()
                if(this.newSeedName === '') {
                    // если данные были стерты, убрать ошибку ввода неверных данных
                    this.newSeedNameError = '';
                }
            }
        },
        newSeedValue2025: {  
            handler() {
                this.checkNewSeedValue()
            }
        },   
        newSeedValue2026: {  
            handler() {
                this.checkNewSeedValue()
            }
        },   
        newSeedValue2027: {  
            handler() {
                this.checkNewSeedValue()
            }
        },   
        newSeedValue2028: {  
            handler() {
                this.checkNewSeedValue()
            }
        },   
    },
    
}
</script>
