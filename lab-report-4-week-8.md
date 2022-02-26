# Week 8 -- Lab Report 4

[Link to index](./index.html)

This report will describe the results of adding and running various markdown test cases to the MarkdownParse.java files of my group and the group whose code we reviewed in week 7.

Note: this report uses [commonmark](https://spec.commonmark.org/dingus/) as the "correct" example.

## Repositories used

[My MarkdownParse](https://github.com/ekaminetzucsd/markdown-parse)
[Other group's MarkdownParse](https://github.com/YueSteveYin/MarkDownParseGroup)

## Test 1

According to commonmark, this test should produce a list of "`google.com", "google.com", and "ucsd.edu".

My implementation with JUnit is below (note: technically, the arguments of `assertEquals` are in the wrong order, but I realized this far too late and don't feel like changing it right now. This will manifest in the output of running failing tests, too):

```
    @Test
    public void testSnippet1() throws IOException {
		String contents = Files.readString(Path.of("snippet1.md"));
		List<String> expect = List.of("`google.com", "google.com", "ucsd.edu");
		assertEquals(MarkdownParse.getLinks(contents), expect);
    }
```

### My implementation

```
1) testSnippet1(MarkdownParseTest)
java.lang.AssertionError: expected:<[url.com, `google.com, google.com]> but was:<[`google.com, google.com, ucsd.edu]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:66)
```

### Other group's implementation

```
1) testSnippet1(MarkdownParseTestGroup)
java.lang.AssertionError: expected:<[url.com, `google.com, google.com]> but was:<[`google.com, google.com, ucsd.edu]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTestGroup.testSnippet1(MarkdownParseTestGroup.java:70)
```

### Potential short code changes

This would be easy enough to implement. In particular, this method:
```
	static int indexOfNoCodeBlock(String markdown, String toFind, int start) {
		boolean inCodeBlock = false;
		for(; markdown,indexOf(toFind, start) != -1; start = markdown.indexOf("`", start)) {
			if(!inCodeBlock && (markdown.indexOf("`", start) < 0 || markdown.indexOf("`", start) > markdown.indexOf(toFind, start))) {
				return markdown.indexOf(toFind, start);
			}
			inCodeBlock = !inCodeBlock;
		}
		return -1;
	}
```

along with changing existing instances of `indexOf()` in the appropriate places (when looking for brackets) to the new method would solve the issue in exactly 10 lines, though the `for` loop is admittedly a bit silly and it would be easier to do in 11 (note: this is untested code that I wrote in like 5-10 minutes so there's a chance it fails, but the gist of it should work)

## Test 2

According to commonmark, this test should produce a list of "a.com", "a.com(())", and "example.com".

My implementation with JUnit is below:

```
    @Test
    public void testSnippet2() throws IOException {
		String contents = Files.readString(Path.of("snippet2.md"));
		List<String> expect = List.of("a.com", "a.com(())", "example.com");
		assertEquals(MarkdownParse.getLinks(contents), expect);
    }
```

### My Implementation

```
2) testSnippet2(MarkdownParseTest)
java.lang.AssertionError: expected:<[a.com, a.com((]> but was:<[a.com, a.com(()), example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:73)
```

### Other group's implementation

```
2) testSnippet2(MarkdownParseTestGroup)
java.lang.AssertionError: expected:<[a.com, a.com((, example.com]> but was:<[a.com, a.com(()), example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTestGroup.testSnippet2(MarkdownParseTestGroup.java:77)
```

### Potential short code changes

For the case of the escaped brackets, we could add the method described in the section on test 1, but with an additional check in the `if` statement with an `&&` that `markdown.charAt(markdown.indexOf(toFind, start)-1) != '\'`, remove the last line and third argument of the `for` loop, and add to the last lines of the `for` loop :

```
start = (markdown.charAt(markdown.indexOf(toFind, start)-1) == "\")?(markdown.indexOf(toFind, start)+1):( markdown.indexOf("`", start);
inCodeBlock = (markdown.charAt(markdown.indexOf(toFind, start)-1) == "\") == inCodeBlock;
```
If given the method added in test 1, this is a rather small change (if not, the method can likely be refactored to still be less than 10 lines but only address escaped brackets).

For the case of the nested parentheses, this will follow an entirely new method similar to the one in test 1, but with an int rather than a boolean counting up when an opening parenthesis is found and down when a closing parenthesis is found. If said int equals zero and a closing parenthesis is found, then it returns that index, though I won't code all of this out here for the sake of brevity.

## Test 3

According to commonmark, this test should produce a list of only "https://ucsd-cse15l-w22.github.io/".

My implementation with JUnit is below:
```
    @Test
    public void testSnippet3() throws IOException {
		String contents = Files.readString(Path.of("snippet3.md"));
		List<String> expect = List.of("https://ucsd-cse15l-w22.github.io/");
		assertEquals(MarkdownParse.getLinks(contents), expect);
    }
```

### My implementation

```
3) testSnippet3(MarkdownParseTest)
java.lang.AssertionError: expected:<[]> but was:<[https://ucsd-cse15l-w22.github.io/]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:80)
```

### Other group's implementation

```
3) testSnippet3(MarkdownParseTestGroup)
java.lang.AssertionError: expected:<[
    https://www.twitter.com
, 
    https://ucsd-cse15l-w22.github.io/
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]> but was:<[https://ucsd-cse15l-w22.github.io/]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at MarkdownParseTestGroup.testSnippet3(MarkdownParseTestGroup.java:84)
```

### Potential short code changes

This one fails specifically because our group excludes links with spaces, though it would also fail if we simply removed this check because it would include spaces and newline characters in the links. However, the fix is still relatively easy; I have to replace the check in my last `if` statement with a check for whether there are at least 2 newline characters between the parentheses (i.e. `markdown.indexOf("\n",markdown.indexOf("\n", openParen)) < closeParen` if no regex) and trim whitespace on the substring that gets added to `toReturn`, i.e. by calling `.trim` on it. I would also have to change the last line of the loop to set currentIndex to one plus the smaller of `markdown.indexOf("\n",markdown.indexOf("\n", openParen))` and `closeParen`.
