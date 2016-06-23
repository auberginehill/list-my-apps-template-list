# example_md


#### Paragraphs
To display a paragraphe, skip one line and type the text. A single line break is a carriage return and does not appear in the export. 

#### Emphasis

#### Bold text

It's been fairly well-tested, and offers advanced features like **bulk enqueueing/dequeueing**
(which, with my new design, is much faster than one element at a time, approaching and even surpassing
the speed of a non-concurrent queue even under heavy contention).

#### Italic text

Reasons *not* to use concurrent data structures.

*** Markdown code ***
*********************

Text _with_ some formatted __elements__ **to _be_** *italic*, **bold** or ~~overlined~~

*** HTML export ***
*******************

Text <em>with</em> some formatted <strong>elements</strong> <strong> to <em>be</em></strong> <em>italic</em>, <strong>bold</strong> or <del>overlined</del>. It mostly just works<sup>TM</sup>, occasionally.

#### Quote block

To display a quote block, start the paragraph with a closing chevron (angle bracket). If your block should contain multiple lines, you can make line breaks by hand and all open by a closing angle bracket, but it is not necessary. This block can contain other elements like titles or lists.

> A quote block is opened by a closing chevron

html:
<blockquote>A quote block is opened by a closing chevron again</blockquote>

#### Display code

To display a block of code, skip one lines as for paragraph, then indent with 4 spaces or a tabulation. The block will end as soon as he arrives on a non-indented line.

Simple example:

    #include "blockingconcurrentqueue.h"
    
    moodycamel::BlockingConcurrentQueue<int> q;
    std::thread producer([&]() {
        for (int i = 0; i != 100; ++i) {
            std::this_thread::sleep_for(std::chrono::milliseconds(i % 10));
            q.enqueue(i);
        }
    });
    std::thread consumer([&]() {
        for (int i = 0; i != 100; ++i) {
            int item;
            q.wait_dequeue(item);
            assert(item == i);
            
            if (q.wait_dequeue_timed(item, std::chrono::milliseconds(5))) {
                ++i;
                assert(item == i);
            }
        }
    });
    producer.join();
    consumer.join();
    
    assert(q.size_approx() == 0);

For explicit producers (using tokens to enqueue):

    (ceil(N / BLOCK_SIZE) + 1) * MAX_NUM_PRODUCERS * BLOCK_SIZ

For implicit producers (no tokens):

    (ceil(N / BLOCK_SIZE) - 1 + 2 * MAX_NUM_PRODUCERS) * BLOCK_SIZE

When using mixed producer types:

    ((ceil(N / BLOCK_SIZE) - 1) * (MAX_EXPLICIT_PRODUCERS + 1) + 2 * (MAX_IMPLICIT_PRODUCERS + MAX_EXPLICIT_PRODUCERS)) * BLOCK_SIZE


*** HTML export ***
*******************

<pre><code>code</code></pre>


#### The inline code font

To display inline `code`, enclose with ` 

Description of basic methods:
`ConcurrentQueue(size_t initialSizeEstimate)`
      Constructor which optionally accepts an estimate of the number of elements the queue will hold
`enqueue(T&& item)`
      Enqueues one item, allocating extra space if necessary
`try_enqueue(T&& item)`
      Enqueues one item, but only if enough memory is already allocated
`try_dequeue(T& item)`
      Dequeues one item, returning true if an item was found or false if the queue appeared empty



Hairline or separator

To display a separator, you have thre options. Write <hr/>, write at least 3 asterisks * or write at least three less sign -. It has to be in a line surrounded by two empty lines. It is possible to separate the signs by one or more spaces. Do what makes the most sense for you and stay consistent in your documents.

*** Markdown code ***
*********************

***
---
- - -
*    *    *

*** HTML export ***
*******************
<hr/>


#### Language color



```
gem install github-markup
```

Usage
-----

```ruby
require 'github/markup'
GitHub::Markup.render('README.markdown', "* One\n* Two")
```

Or, more realistically:

```ruby
require 'github/markup'
GitHub::Markup.render(file, File.read(file))


#### Ordered list

To display an ordered list, start the line with a number followed by a period. 

1. Use the bulk methods of the queue with tokens
2. Failing that, use the bulk methods without tokens
3. Failing that, use the single-item methods with tokens
4. Failing that, use the single-item methods without tokens

#### Unordered list

To display a list, begin the line with an asterisk * less - or a +. 

- Enqueue operations are rolled back completely if an exception is thrown from an element's constructor.
  For bulk enqueue operations, this means that elements are copied instead of moved (in order to avoid
  having only some of the objects be moved in the event of an exception). Non-bulk enqueues always use
  the move constructor if one is available.
- If the assignment operator throws during a dequeue operation (both single and bulk), the element(s) are
  considered dequeued regardless. In such a case, the dequeued elements are all properly destructed before
  the exception is propagated, but there's no way to get the elements themselves back.
- Any exception that is thrown is propagated up the call stack, at which point the queue is in a consistent
  state.

#### Link to file

There are some more detailed samples [here][samples.md]. The source of
the [unit tests][unittest-src] and [benchmarks][benchmark-src] are available for reference as well.

See [Contributing](CONTRIBUTING.md)

#### Link to url

Link sample: [ChakraCore Architecture](https://github.com/Microsoft/ChakraCore/wiki/Architecture-Overview)

*** Markdown code ***
*********************

<http://www.google.com>


Images

To display an image, start by an exclamation point. Then specify the alternative text between brackets (this will be displayed if the image is not loaded and read by the search engines). Finish with image URL between parentheses. This URL can be a link to the web or a local path. After this picture, it is possible to add a title read by text browsers and displayed over the image by others.

*** Markdown code ***
*********************

![Google logo](https://www.google.fr/images/srpr/logo11w.png "google logo")

*** HTML export ***
*******************

<img src = "https://www.google.fr/images/srpr/logo11w.png" title = "google logo" alt = "Google logo">




tables

The tables do not exist in the original markdown specification, but they are present in almost all recent implementations. And it really has a very high value in some cases.

The idea is to “draw” columns by surrounding them with pipes |. The number of columns defined in the first row and for each row you must have the same number of columns, even if some are empty.

The first line will be your header. The second line separates the header from the body of the table and sets the alignment of text in columns. It contains only the dash - and colon(s) :. Colons are used to define the text alignment. No : or just on at left means that the text will be left-aligned. If the line of - is surrounded by 2 :, the text will be centered and with only one : in the right of a line, the text will be right-aligned.

*** Markdown code ***
*********************

| Header 1      |     2 header    |   header 3 |
| ------------- | :-------------: | ---------: |
| Line 1        |        1        |    a value |
| Line 2        |        2        |    b value |
| Line 3        |        3        |    c value |


The | starting and ending the lines are optional. It is possible to embed elements of emphasis or code in the tables. There is need of only one - per column for the separation between the header and the body of the table.




Escaping characters

Special characters with a speacial application in HTML or markdown must be escaped.For ampersands &amp;, angle brackets < and other HTML characters markdown is handle the convertion in HTML entities when exporting. But if you want to use asterisks, brackets, sharps… in your text that could be markdown formatters, you need to escape them by preceding them with a backslash \. Markdown if the mask and apply the appropriate formatting. The following characters are escaped:

\ * `- _ [] () {} + #. !



*** Markdown code ***
*********************

> Quote 1

<!-- comment text -->

> Quote 2



[![Join the chat at https://gitter.im/Microsoft/ChakraCore](https://badges.gitter.im/Microsoft/ChakraCore.svg)](https://gitter.im/Microsoft/ChakraCore?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

ChakraCore is the core part of Chakra, the high-performance JavaScript engine that powers Microsoft Edge and Windows applications written in HTML/CSS/JS.  ChakraCore supports Just-in-time (JIT) compilation of JavaScript for x86/x64/ARM, garbage collection, and a wide range of the latest JavaScript features.  ChakraCore also supports the [JavaScript Runtime (JSRT) APIs](https://github.com/Microsoft/ChakraCore/wiki/JavaScript-Runtime-%28JSRT%29-Overview), which allows you to easily embed ChakraCore in your applications.

You can stay up-to-date on progress by following the [MSEdge developer blog](https://blogs.windows.com/msedgedev/).

## [Build Status](https://github.com/Microsoft/ChakraCore/wiki/Build-Status)

|         | __Debug__ | __Test__ | __Release__ |
|:-------:|:---------:|:--------:|:-----------:|
| __x86__ | [![x86debug][x86dbgicon]][x86dbglink] | [![x86test][x86testicon]][x86testlink] | [![x86release][x86relicon]][x86rellink] |
| __x64__ | [![x64debug][x64dbgicon]][x64dbglink] | [![x64test][x64testicon]][x64testlink] | [![x64release][x64relicon]][x64rellink] |
| __arm__ | [![armdebug][armdbgicon]][armdbglink] | [![armtest][armtesticon]][armtestlink] | [![armrelease][armrelicon]][armrellink] |
| __linux__ | [![linuxdebug][linuxdbgicon]][linuxdbglink] | N/A | [![linuxrelease][linuxrelicon]][linuxrellink] |

*If you see badges reading "Build: Unknown" it is likely because a build was skipped due to changes being only in files known not to affect the health of the build.*

[x86dbgicon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x86_debug/badge/icon
[x86dbglink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x86_debug/
[x86testicon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x86_test/badge/icon
[x86testlink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x86_test/
[x86relicon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x86_release/badge/icon
[x86rellink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x86_release/

[x64dbgicon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x64_debug/badge/icon
[x64dbglink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x64_debug/
[x64testicon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x64_test/badge/icon
[x64testlink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x64_test/
[x64relicon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x64_release/badge/icon
[x64rellink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/x64_release/

[armdbgicon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/arm_debug/badge/icon
[armdbglink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/arm_debug/
[armtesticon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/arm_test/badge/icon
[armtestlink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/arm_test/
[armrelicon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/arm_release/badge/icon
[armrellink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/arm_release/

[linuxdbgicon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/ubuntu_linux_debug/badge/icon
[linuxdbglink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/ubuntu_linux_debug
[linuxrelicon]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/ubuntu_linux_release/badge/icon
[linuxrellink]: http://dotnet-ci.cloudapp.net/job/Microsoft_ChakraCore/job/master/job/ubuntu_linux_release/

Above is a table of our rolling build status. We run additional builds on a daily basis. See [Build Status](https://github.com/Microsoft/ChakraCore/wiki/Build-Status) for the status of all builds.

## Security

If you believe you have found a security issue in ChakraCore, please share it with us privately following the guidance at the Microsoft [Security TechCenter](https://technet.microsoft.com/en-us/security/ff852094). Reporting it via this channel helps minimize risk to projects built with ChakraCore.

## Documentation

* [ChakraCore Architecture](https://github.com/Microsoft/ChakraCore/wiki/Architecture-Overview)
* [Quickstart Embedding ChakraCore](https://github.com/Microsoft/ChakraCore/wiki/Embedding-ChakraCore)
* [JSRT Reference](https://github.com/Microsoft/ChakraCore/wiki/JavaScript-Runtime-%28JSRT%29-Reference)
* [Contribution guidelines](CONTRIBUTING.md)
* [Blogs, talks and other resources](https://github.com/Microsoft/ChakraCore/wiki/Resources)

## Housekeeping

Your pull request should:
* Include a description of what your change intends to do
* Be a child commit of a reasonably recent commit in the master branch
* Pass all unit tests
* Have a clear commit message
* Include adequate tests
  * At least one test should fail in the absence of your non-test code changes. If your PR does not match this criteria, please specify why
  * Tests should include reasonable permutations of the target fix/change
  * Include baseline changes with your change

Submissions that have met these requirements will be assigned to a ChakraCore team member for additional testing. Submissions must meet functional and performance expectations, including meeting requirements in scenarios for which the team doesn’t yet have open source tests. This means you may be asked to fix and resubmit your pull request against a new open test case if it fails one of these tests. The ChakraCore team may verify your change by crawling the web with your change built into Chakra. Failures discovered when testing with this technique will not be analyzed by the team, but we will do our best to communicate the issue discovered to you. This approach needs further refinement, we acknowledge.

ChakraCore is an organically grown codebase. The consistency of style reflects this. For the most part, the team follows these [coding conventions](https://github.com/Microsoft/ChakraCore/wiki/Coding-Convention). Contributors should also follow them when making submissions. Otherwise, follow the general coding conventions adhered to in the code surrounding your changes. Pull requests that reformat the code will not be accepted.

## Building ChakraCore

You can build ChakraCore on Windows 7 SP1 or above, and Windows Server 2008 R2 or above, with either Visual Studio 2013 or 2015 with C++ support installed.  Once you have Visual Studio installed:

* Clone ChakraCore through ```git clone https://github.com/Microsoft/ChakraCore.git```
* Open `Build\Chakra.Core.sln` in Visual Studio
* Build Solution

More details in [Building ChakraCore](https://github.com/Microsoft/ChakraCore/wiki/Building-ChakraCore).

## Using ChakraCore

Once built, you have a few options for how you can use ChakraCore:

* The most basic is to test the engine is running correctly with the *ch.exe* binary.  This app is a lightweight hosting of JSRT that you can use to run small applications.  After building, you can find this binary in:
  * `Build\VcBuild\bin\${platform}_${configuration}`
  * (e.g. `Build\VcBuild\bin\x64_debug`)
* You can [embed ChakraCore](https://github.com/Microsoft/ChakraCore/wiki/Embedding-ChakraCore) in your applications - see [documentation](https://github.com/Microsoft/ChakraCore/wiki/Embedding-ChakraCore) and [samples](http://aka.ms/chakracoresamples).
* Finally, you can also use ChakraCore as the JavaScript engine in Node.  You can learn more by reading how to use [Chakra as Node's JS engine](https://github.com/Microsoft/node)

_A note about using ChakraCore_: ChakraCore is the foundational JavaScript engine, but it does not include the external APIs that make up the modern JavaScript development experience.  For example, DOM APIs like ```document.write()``` are additional APIs that are not available by default and would need to be provided.  For debugging, you may instead want to use ```print()```.

## Contribute

Contributions to ChakraCore are welcome.  Here is how you can contribute to ChakraCore:

* [Submit bugs](https://github.com/Microsoft/ChakraCore/issues) and help us verify fixes
* [Submit pull requests](https://github.com/Microsoft/ChakraCore/pulls) for bug fixes and features and discuss existing proposals
* Chat about [@ChakraCore](https://twitter.com/ChakraCore) on Twitter

Please refer to [Contribution guidelines](CONTRIBUTING.md) and the [Code of Conduct](CODE_OF_CONDUCT.md) for more details.

## Roadmap
For details on our planned features and future direction please refer to our [roadmap](https://github.com/Microsoft/ChakraCore/wiki/Roadmap).

## Contact us
For questions about ChakraCore, you can reach us on [Gitter](https://gitter.im/Microsoft/ChakraCore) or open an [issue](https://github.com/Microsoft/ChakraCore/issues/new) and prefix the issue title with [Question]. See [Question](https://github.com/Microsoft/ChakraCore/issues?q=label%3AQuestion) tag for already-opened questions.



source: <http://blog.wax-o.com/2014/04/tutorial-short-guide-to-start-with-markdown/>
