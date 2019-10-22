#Introduction to TDD


-
-

## What we'll cover
<ul>
<li class="fragment fade-up">What is TDD?</li>
<li class="fragment fade-up">3 laws of TDD</li>
<li class="fragment fade-up">Characteristics of a clean test.</li>
<li class="fragment fade-up">F.I.R.S.T.</li>
</ul>

-
-
#3 laws of TDD

###First Law

You may not write production code until you have a written a failing corresponding unit test.

```
	public class cow{
		public cow(){}
		
		public String speak(){
			//BAD DOBBY
			return "Mooooow";
		}
	}

```

-
-
###First Law

You may not write production code until you have a written a failing corresponding unit test.

```
public class cow{
	public cow(){}
	
	public String speak(){
		//Good DOBBY
		return null;
	}
}

```
-

```
public class cowTest{
	
	@Test
	public void speakTest(){
		Cow cow = new Cow();
		String expected = "mooo";
		String actual = cow.speak();
		Assert.equals(expected, actual);
	}
}
```
-
#3 laws of TDD

###Second Law

You may not write more of a unit test than is needed to fail, and not compiling is failing.

```
@Test
public void speakTest(){
	Cow cow = new Cow();
	Cow cow2 = new Cow();
	
	String expected = "mooo";
	String actual = cow.speak();
	
	String expected2 = "mooo";
	String actual2 = cow2.speak();
	Assert.equals(expected2, actual2);
	// BAD DOBBY
	Assert.equals(actual, actual2);
}
```

-
#3 laws of TDD

###Third Law

You may not write more production code than is sufficent to pass current failing test.

```
public String speak(){
	String sound = "Mooooo";
	//BAD DOBBY!!!!
	String bucketsOfMilk = this.milkMe();
	return sound;
}
```

-
-

#Clean Code and Clean Test

A clean test is a readable Test

```
@Test
public void testFunction(){
//Bad Dobby
Unicorn unico = New Unicorn();
int x = 23;
int y = unico.saveThePrincess();
Assert.equals(x,y);
}

```

-
```
@Test
public void testCountToTwo(){
//Good Dobby
Unicorn unico = New Unicorn();
int expectedNumber = 2;
int actualNumber = unico.countToTwo();
Assert.equals(expectedNumber, actualNumber);
}
```
-
-
#One Assertion to Rule them All

A good unit test should only come to one binary conclusion, which should also be quick and easy to understand. 

**Remember: SINGLE RESPONSIBLITY**

Single Concept per test, with the following:
<ul>
<li class="fragment fade-up">Given (Sets the stage)</li>
<li class="fragment fade-up">When (Performs the action)</li>
<li class="fragment fade-up">Evaluate (Checks the results)</li>
</uL>

-
-
#F.I.R.S.T

<ul>
<li class="fragment fade-up"> **F** is for **fast** - The test should be fast. If its going to fail lets get it over with.</li>
<li class="fragment fade-up"> **I** is for **Independent** - every test is an island a "LONELY ISLAND". The the test should not have to fire off other methods to complete itself.</li>
<li class="fragment fade-up"> **R** is for **Repeatable** - every test should be able to run in any environment , local, QA, and Production.</li>
<li class="fragment fade-up"> **S** is for **Self-Validating** - every test should have a binary boolean output pass or fail.</li>
<li class="fragment fade-up"> **T** is for **Timely** - test should be written FIRST... OR ELSE!!! </li>
</ul>
