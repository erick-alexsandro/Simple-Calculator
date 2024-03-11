<template>
  <div id="mydiv" class="calculator">
    <div id="mydivheader" class="display">{{ current || 0 }}</div>
    <div @click="clear" class="button">C</div>
    <div @click="sign" class="button">+/-</div>
    <div @click="percent" class="button">%</div>
    <div @click="divide" class="button operator">÷</div>
    <div @click="append('7')" class="button">7</div>
    <div @click="append('8')" class="button">8</div>
    <div @click="append('9')" class="button">9</div>
    <div @click="multiply" class="button operator">x</div>
    <div @click="append('4')" class="button">4</div>
    <div @click="append('5')" class="button">5</div>
    <div @click="append('6')" class="button">6</div>
    <div @click="subtract" class="button operator">-</div>
    <div @click="append('1')" class="button">1</div>
    <div @click="append('2')" class="button">2</div>
    <div @click="append('3')" class="button">3</div>
    <div @click="add" class="button operator">+</div>
    <div @click="append('0')" class="zero button">0</div>
    <div @click="dot" class="button">.</div>
    <div @click="equal" class="button operator">=</div>
  </div>
  <div
    v-show="maxLength === true"
    id="alert"
    class="alert alert-danger"
    role="alert"
  >
    <strong>Can't enter more than 15 digits!</strong>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from "vue";

let current = ref("");
let previous = ref("");
let operator = ref(false);
let operatorClicked = ref(false);
let maxLength = ref(false);

watch(current, () => {
  if (current.value.length === 15) {
    maxLength = true;
  } else {
    maxLength = false;
  }
});

function clear() {
  current.value = "";
}
function sign() {
  if (current.value != "") {
    current.value =
      current.value.charAt(0) === "-"
        ? current.value.slice(1)
        : `-${current.value}`;
  }
}

function percent() {
  if (current.value != "") {
    current.value = `${parseFloat(current.value) / 100}`;
  }
}

function append(number) {
  if (current.value.length < 15) {
    if (operatorClicked.value === true) {
      current.value = "";
      operatorClicked.value = false;
    }
    current.value = `${current.value}${number}`;
  }
}

function dot() {
  if (current.value.indexOf(".") === -1) {
    current.value = current.value + ".";
  }
}

function setPrevious() {
  previous.value = current.value;
  operatorClicked.value = true;
}

function divide() {
  operator.value = (a, b) => a / b;
  setPrevious();
}

function multiply() {
  operator.value = (a, b) => a * b;
  setPrevious();
}

function subtract() {
  operator.value = (a, b) => a - b;
  setPrevious();
}

function add() {
  operator.value = (a, b) => a + b;
  setPrevious();
}

function equal() {
  current.value = `${operator.value(
    parseFloat(previous.value),
    parseFloat(current.value)
  )}`;
  previous.value = "";
}

onMounted(() => {
  dragElement(document.getElementById("mydiv"));
  let divHeader = document.getElementById("mydivheader");

  function dragElement(elmnt) {
    var pos1 = 0,
      pos2 = 0,
      pos3 = 0,
      pos4 = 0;
    if (document.getElementById(elmnt.id + "header")) {
      document.getElementById(elmnt.id + "header").onmousedown = dragMouseDown;
    } else {
      elmnt.onmousedown = dragMouseDown;
    }

    function dragMouseDown(e) {
      e = e || window.event;
      e.preventDefault();
      pos3 = e.clientX;
      pos4 = e.clientY;
      document.onmouseup = closeDragElement;
      document.onmousemove = elementDrag;
      divHeader.classList.add("grabbing");
    }

    function elementDrag(e) {
      e = e || window.event;
      e.preventDefault();
      pos1 = pos3 - e.clientX;
      pos2 = pos4 - e.clientY;
      pos3 = e.clientX;
      pos4 = e.clientY;
      elmnt.style.top = elmnt.offsetTop - pos2 + "px";
      elmnt.style.left = elmnt.offsetLeft - pos1 + "px";
    }

    function closeDragElement() {
      document.onmouseup = null;
      document.onmousemove = null;
      divHeader.classList.remove("grabbing");
    }
  }
});
</script>

<style>
.grabbing {
  cursor: grabbing !important;
}
#alert {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  margin: 0 auto;
  opacity: 70%;
}
#mydiv {
  position: absolute;
  z-index: 9;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

#mydivheader {
  cursor: grab;
  z-index: 10;
}
.calculator {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  font-size: 40px !important;
  width: 350px;
  height: 450px;
  margin: 0 auto;
  border-radius: 15px;
  overflow: hidden;
  border: 1px solid #fbfbfe;
}
.display {
  grid-column: 1 / 5;
  background: #020024;
  color: #fbfbfe;
  display: flex;
  justify-content: right;
  align-items: center;
  padding: 10px;
}

.zero {
  grid-column: 1/3;
}

.button {
  background: #ebe9fc;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
}

.button:hover {
  filter: brightness(80%) !important;
  cursor: pointer;
}

.operator {
  background: #3a31d8;
  color: white;
}
</style>
