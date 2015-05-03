---
layout: post
title:  "Mukluk: A Dark Theme For Pygments"
date:   2015-03-01
summary: Mukluk is a dark pygments/rouge theme created for my Jekyll blog. It has support for multiple programming languages.
categories: bootflat jekyll pygments rouge theme
---

When I decided to split my blog into two blogs (technical and non-technical), I
realized that I would need to have good code syntax highlighting for my
technical blog (this one). I developed this theme for pygments/rouge based on
the [bootflat](https://bootflat.github.io/documentation.html "Bootflat") color
scheme.

I'm calling it "Mukluk" because they are boots with flat soles (and it is a fun
word to say).

Here is the sass file I use:

{% highlight sass %}
/**
 * Syntax highlighting styles
   Mukluk, based on bootflat
   https://bootflat.github.io/documentation.html
 */
$blue-jeans  : #5D9CEC;
$aqua        : #4FC1E9;
$mint        : #48CFAD;
$grass       : #A0D468;
$sunflower   : #FFCE54;
$bittersweet : #FC6E51;
$grapefruit  : #ED5565;
$lavendar    : #AC92EC;
$pink-rose   : #EC87C0;
$light-gray  : #F5F7FA;
$medium-gray : #CCD1D9;
$gray        : #656D78;
$dark-gray   : #434A54;

.highlight {
  color: $medium-gray;
  line-height: 1.4em;
  background: $dark-gray;
  border-radius: 5px;
  padding: 1em;
  margin: 1em 0;

  .c     { color: $medium-gray; font-style: italic } // Comment
  .err   { color: $grapefruit; } // Error
  .k     { color: $bittersweet; } // Keyword
  .o     { color: $light-gray } // Operator
  .cm    { color: $gray; font-style: italic } // Comment.Multiline
  .cp    { color: $gray; font-weight: bold } // Comment.Preproc
  .c1    { color: $gray; font-style: italic } // Comment.Single
  .cs    { color: $gray; font-weight: bold; font-style: italic } // Comment.Special
  .gd    { color: $gray; background-color: $medium-gray } // Generic.Deleted
  .gd .x { color: $gray; background-color: $medium-gray } // Generic.Deleted.Specific
  .ge    { font-style: italic } // Generic.Emph
  .gr    { color: $lavendar } // Generic.Error
  .gh    { color: $gray } // Generic.Heading
  .gi    { color: $dark-gray; background-color: $medium-gray } // Generic.Inserted
  .gi .x { color: $dark-gray; background-color: $medium-gray } // Generic.Inserted.Specific
  .go    { color: $gray } // Generic.Output
  .gp    { color: $gray } // Generic.Prompt
  .gs    { font-weight: bold } // Generic.Strong
  .gu    { color: $light-gray } // Generic.Subheading
  .gt    { color: $grapefruit } // Generic.Traceback
  .kc    { color: $sunflower } // Keyword.Constant
  .kd    { color: $sunflower } // Keyword.Declaration
  .kp    { color: $sunflower } // Keyword.Pseudo
  .kr    { color: $sunflower } // Keyword.Reserved
  .kt    { color: $blue-jeans } // Keyword.Type
  .m     { color: $grapefruit } // Literal.Number
  .s     { color: $grass } // Literal.String
  .na    { color: $aqua } // Name.Attribute
  .nb    { color: $mint } // Name.Builtin
  .nc    { color: $grapefruit } // Name.Class
  .no    { color: $grapefruit } // Name.Constant
  .ni    { color: $grapefruit } // Name.Entity
  .ne    { color: $light-gray } // Name.Exception
  .nf    { color: $sunflower } // Name.Function
  .nn    { color: $medium-gray } // Name.Namespace
  .nt    { color: $blue-jeans } // Name.Tag
  .nv    { color: $aqua } // Name.Variable
  .ow    { font-weight: bold } // Operator.Word
  .w     { color: $light-gray } // Text.Whitespace
  .mf    { color: $blue-jeans } // Literal.Number.Float
  .mh    { color: $blue-jeans } // Literal.Number.Hex
  .mi    { color: $blue-jeans } // Literal.Number.Integer
  .mo    { color: $blue-jeans } // Literal.Number.Oct
  .sb    { color: $grass } // Literal.String.Backtick
  .sc    { color: $grass } // Literal.String.Char
  .sd    { color: $grass } // Literal.String.Doc
  .s2    { color: $grass } // Literal.String.Double
  .se    { color: $grass } // Literal.String.Escape
  .sh    { color: $grass } // Literal.String.Heredoc
  .si    { color: $grass } // Literal.String.Interpol
  .sx    { color: $grass } // Literal.String.Other
  .sr    { color: $pink-rose } // Literal.String.Regex
  .s1    { color: $grass } // Literal.String.Single
  .ss    { color: $lavendar } // Literal.String.Symbol
  .bp    { color: $medium-gray } // Name.Builtin.Pseudo
  .vc    { color: $grapefruit } // Name.Variable.Class
  .vg    { color: $grapefruit } // Name.Variable.Global
  .vi    { color: $aqua } // Name.Variable.Instance
  .il    { color: $blue-jeans } // Literal.Number.Integer.Long

  .lineno { color: $gray }
}
{% endhighlight %}

Here are some example of the syntax highlighting in various languages:

{% highlight ruby %}
# ruby
module Foo
  def foo
    puts "foo"
  end
end

class Bar < Baz
  include Foo
  attr :quux

  def initialize(options)
    @options = options || {}
  end

  def types
    num = 10_000
    ber = 1234.5678
    3.times do
      puts num
    end
    string = "Hello World"
    range  = 'a'..'z'
    regex  = /foobar/
  end

  private

  def collections
    ages         = {bob: 20, bill: 32, jane: 19}
    hash_rockets = {:foo => "bar", :baz => "quux"}
  end
end
{% endhighlight %}

{% highlight javascript linenos %}
// javascript
$(document).ready(function() {
  var foo = "bar";
  window.foo = foo;

  var baz = {
    quux: function(corge) {
      console.log(corge);
      return corge;
    },
    _grault: 1234
  }
});
{% endhighlight %}

{% highlight lua %}
-- lua
local a = require('mod2')
local b = require('mod2')

function fib(n)
  if n < 2 then return 1 end
  return fib(n - 2) + fib(n - 1)
end

t = {key1 = 'value1', key2 = false}
print(t.key1)
t.newKey = {}
t.key2 = nil

{% endhighlight %}

{% highlight coffeescript linenos %}
# coffeescript
if this.studyingEconomics
  buy()  while supply > demand
  sell() until supply > demand

num = 6
lyrics = while num -= 1
  "#{num} little monkeys, jumping on the bed.
    One fell out and bumped his head."
{% endhighlight %}
