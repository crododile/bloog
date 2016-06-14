---
layout: post
title: Counting in English in SCSS
categories: CSS FrontEnd
---
I recently had a requirement to list a collection of "steps" in a process with English labels (i.e. Step one, Step two, Step three). The initial version of this listing used the [Humanize Gem](https://github.com/radar/humanize) to convert the index of the steps into the english pronunciation during templating. I was not very happy with this solution because in classic Ruby fashion the Humanize Gem monkey patches Numeric to add this behavior. Kinda gross. Later, the feature evolved to require adding steps dynamically with Javascript. The templating version would no longer suffice. Rather than track or implement a Javascript library to match the Humanize way, I knew there had to be a way to accomplish this effect with SCSS. I am happy with the outcome as it does not have any extra dependencies and solves the problem of having to add the human readable versions of the number in two places. The code for the mixin follows.

~~~ scss
/* USAGE: extend to generate "after" content containing the english count of nested elements with class "countable" */
/* div.contains-countable { */
/*   @extend %contains-countable; */
/* } */

/* <div class='contains-countable'> */
/* <div class='countable'>#{Will render "one" in :after }</div> */
/* <div class='countable'>#{Will render "two" in :after }</div> */
/* </div> */

%contains-countable{
  $numerals: 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine', 'ten', 'eleven', 'twelve', 'thirteen', 'fourteen', 'fifteen', 'sixteen', 'seventeen', 'eighteen', 'nineteen', 'twenty', 'twenty-one', 'twenty-two', 'twenty-three', 'twenty-four', 'twenty-five', 'twenty-six', 'twenty-seven', 'twenty-eight', 'twenty-nine', 'thirty', 'thirty-one', 'thirty-two', 'thirty-three', 'thirty-four', 'thirty-five', 'thirty-six', 'thirty-seven', 'thirty-eight', 'thirty-nine', 'forty', 'forty-one', 'forty-two', 'forty-three', 'forty-four', 'forty-five', 'forty-six', 'forty-seven', 'forty-eight', 'forty-nine', 'fifty', 'fifty-one', 'fifty-two', 'fifty-three', 'fifty-four', 'fifty-five', 'fifty-six', 'fifty-seven', 'fifty-eight', 'fifty-nine', 'sixty', 'sixty-one', 'sixty-two', 'sixty-three', 'sixty-four', 'sixty-five', 'sixty-six', 'sixty-seven', 'sixty-eight', 'sixty-nine', 'seventy', 'seventy-one', 'seventy-two', 'seventy-three', 'seventy-four', 'seventy-five', 'seventy-six', 'seventy-seven', 'seventy-eight', 'seventy-nine', 'eighty', 'eighty-one', 'eighty-two', 'eighty-three', 'eighty-four', 'eighty-five', 'eighty-six', 'eighty-seven', 'eighty-eight', 'eighty-nine', 'ninety', 'ninety-one', 'ninety-two', 'ninety-three', 'ninety-four', 'ninety-five', 'ninety-six', 'ninety-seven', 'ninety-eight', 'ninety-nine';
  $elements: length($numerals);

  @for $i from 1 to $elements {
    &:nth-of-type(#{$i}){
      .countable::after{ content: nth($numerals, $i);}
    }
  }
}

~~~
