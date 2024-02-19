<script setup>
// computed - исп. для обьявл. реактив. сво-в. вычисляется когда измен. их зависимости
import { ref, computed } from 'vue'

//в начале на экране 0-ль
const display = ref("0")

//функция добавления на экран, там в строковом типе
const appendToDisplay = (value) => {
    if (display.value === '0' && value !== ".") {
        display.value = value
    } else {
        display.value += value;
    }
}

//выполнить матем. выражение с помощью eval
const calculate = () => {
    try {
        //eval - выполняет выражение которое предст. в виде строки. toString() -в строку
        display.value = eval(display.value).toString()
    } catch (error) {
        display.value = 'Error'
    }
}

// Computed property for dynamic class binding
/* Функция создаёт вычисляемое свойство displayClass, которое динамически вычисляет
и возвращает имя класса на основе длины строки, содержащейся в реактивном свойстве
display.value.
Давайте разберём её подробнее:
    computed:
        Это функция из Vue.js, которая создаёт вычисляемое свойство. Вычисляемые
        свойства реактивны и автоматически пересчитываются, когда изменяются
        реактивные данные, от которых они зависят.
    display.value:
        Это обращение к реактивному свойству (скорее всего, объявленному
        с помощью ref или reactive из Composition API Vue.js). value является способом
        доступа к значению реактивного свойства, объявленного через ref.
    display.value.length > 12:
        Это условие проверяет, превышает ли длина строки, содержащейся в display.value,
        12 символов.
    В зависимости от условия, функция возвращает "small-text", если длина строки больше
    12 символов. В противном случае, возвращается пустая строка.
Зачем это нужно?
    Использование такого вычисляемого свойства позволяет динамически изменять класс
    элемента в пользовательском интерфейсе на основе содержимого реактивного свойства.
    В данном случае, если текст длинный (более 12 символов), для элемента может быть
    применён CSS-класс small-text, который, вероятно, уменьшает размер шрифта  */
const displayClass = computed(() => {
    return display.value.length > 12 ? "small-text" : "";
});

// функция очищения дисплея
const clearDisplay = () => {
    display.value = "0";
};

</script>

<template>
    <div>
        <div class="calculator">
            <!-- v-model="display":
                    Директива v-model создаёт двустороннюю привязку между значением
                    элемента формы (в данном случае, текстового поля ввода) и
                    переменной display во Vue-компоненте. Это означает, что любые
                    изменения в поле ввода автоматически обновят переменную display в
                    данных компонента, и наоборот, любые изменения в display будут
                    отражаться в поле ввода. Это очень удобно для создания
                    интерактивных форм.
                :class="displayClass":
                    Это привязка класса с использованием Vue. displayClass должно
                    быть именем класса или условием, возвращаемым вычисляемым
                    свойством (как в предыдущем примере с функцией). В зависимости
                    от условий, указанных в displayClass, к элементу ввода будет
                    динамически применён соответствующий класс CSS. Например, если
                    длина текста в поле ввода превышает определённый порог, может
                    быть применён класс, изменяющий стиль текста или самого элемента.
                readonly: 
                    Этот атрибут указывает, что поле ввода только для чтения и
                    не может быть изменено пользователем. Это полезно, когда вы хотите
                    отобразить значение переменной display без возможности его изменения
                    со стороны пользователя.
                В совокупности, этот код создаёт элемент ввода, который отображает
                данные, связанные с переменной display в вашем Vue-компоненте,
                позволяет динамически применять CSS-классы на основе этих данных
                (через displayClass) и делает поле ввода только для чтения, исключая
                возможность его изменения пользователем.  -->
            <input v-model="display" :class="displayClass" readonly />

            <div class="buttons">
                <button @click="appendToDisplay('7')">7</button>
                <button @click="appendToDisplay('8')">8</button>
                <button @click="appendToDisplay('9')">9</button>
                <button @click="appendToDisplay('/')">/</button>

                <button @click="appendToDisplay('4')">4</button>
                <button @click="appendToDisplay('5')">5</button>
                <button @click="appendToDisplay('6')">6</button>
                <button @click="appendToDisplay('*')">*</button>

                <button @click="appendToDisplay('1')">1</button>
                <button @click="appendToDisplay('2')">2</button>
                <button @click="appendToDisplay('3')">3</button>
                <button @click="appendToDisplay('-')">-</button>

                <button @click="appendToDisplay('0')">0</button>
                <button @click="appendToDisplay('.')">.</button>
                <button @click="calculate()">=</button>
                <button @click="appendToDisplay('+')">+</button>
            </div>

            <button @click="clearDisplay" class="clear-button">C</button>
        </div>
    </div>
</template>

<style>
input {
    padding: 10px 20px;
    margin-bottom: 20px;
}

.calculator {
    max-width: 300px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
}
 
.display {
    width: 100%;
    margin-bottom: 10px;
    padding: 10px;
    font-size: 18px;
    text-align: right;
}

.buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
}

button {
    width: 100%;
    padding: 10px;
    font-size: 18px;
}

.clear-button {
    width: 100%;
    margin-top: 10px;
}

.small-text {
    font-size: 14px;
}
</style>