# 每周总结可以写在这里
String Object
内部使用 [[StringData]] 存储原始字符串值
字符串提供了一个 length 属性来访问其长度



function convertNumberTostr(number, x = 10) {
    var integer = Math.floor(number);
    var fraction = null;
    if (x === 10) fraction = ('' + number).match(/\.\d*/)[0];
    var str = ''
    while (integer > 0) {
        str = integer % x + str;
        integer = Math.floor(integer / x);
    }
    return fraction ? str + fraction : str;
}

function convertstrToNumber(str, x) {
  if (arguments.length < 2) {
    x = 10;
  }
  let letters = ["a", "b", "c", "d", "e", "f"];
  let chars = str.toLowerCase().split("");
  let flag = chars.includes("-");
  let number = 0;
  let i = 0;

  while (i < chars.length && chars[i] !== "." && !letters.includes(chars[i])) {
    number *= x;
    number += chars[i].codePointAt() - "0".codePointAt();
    i++;
  }

  // Point
  if (chars[i] === ".") {
    i++;
  }
  let fraction = 1;
  while (
    x === 10 &&
    i < chars.length &&
    chars[i] !== "e" &&
    chars[i] !== "+" &&
    chars[i] !== "-"
  ) {
    fraction /= x;
    number += (chars[i].codePointAt() - "0".codePointAt()) * fraction;
    i++;
  }

  // index
  if ((x === 10 && chars[i] === "-") || chars[i] === "+" || chars[i] === "e") {
    i++;
  }
  let index = 0;
  while (x === 10 && i < chars.length) {
    index *= x;
    index += convertstrToNumber(chars[i]);
    if (flag) number /= x ** index;
    else number *= x ** index;
    i++;
  }

  //  hex
  while (x === 16 && letters.includes(chars[i])) {
    number *= x;
    number += chars[i].codePointAt() - 87; // a 97
    i++;
  }
  return number;
}