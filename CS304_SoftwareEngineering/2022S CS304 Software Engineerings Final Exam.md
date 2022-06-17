# 2022S CS304 SE Final Exam

## Intro

**Exam scheduler**: Shin hweitan
**Reviewer**： Wen Yingtong ([TungMan0801](https://github.com/TungMan0801))，Yi Xiang ([Yxx](https://github.com/Sustech-yx)), who both are 2019 grade in SUSTech and learning SE in 2022S. This semester has no mid-term.

❗The review is only for directing reviewing for younger students and memorizing how much we learn. Remember that the content appearing in only one exam can not cover all knowledge that you should learn. Please don't see it as a standard.
The following exam content is from our memory after taking exams.:)

## Question Type

- 20 multiple choice (only one answer is correct).
- Essay Questions
  - Test Coding(Write Junit test)
  - Security(Juding which security type is invoked)


## Multiple choice
### Invoking Knowledgement
Pokemon theme. The exam maker must be a big fan.(Actually only some questions are about it.)
The knowledge that may be coverred..
- XP's practice
- UI design rules
- Metrics
- javadoc format
- tools in this course
- Testing
- Security
- ...


#### T1
Maybe some details are not right... But the coding question will look like this. 
```java=
private static final String MARKER_AT = "(at";
private static final String MARKER_ASSERT = "/Asserts/";
public static String op(String cr){
    int start=cr.indexOf(MARKER_AT);
    if(start==-1)return cr;
    int end=cr.indexOf(MARKER_ASSERT);
    return end!=-1?cr.substring(0,start+MARKER_AT.length())+cr.substring(end):cr;
}
```
**Q**: Which statement causes a **error** but does **NOT** cause a **failure**?
A.op(null) 
B.op(new String("")) 
C.op(new String("(at/home/ASSERT)")) 
D.op(new String("Exception (at/home/ASSERT)"))

**Q:** In which line there is a fault...

(It's how a question will look like. But in this piece of code, there is no fault. )


#### T2
Which tool you can use to check coding standard?//do mutation tests?...
#### T3
1. Which following javadoc description is in right format?
A.Return the value of XX
B.Returns the value of XX
C.This method returns the xxx type xxx....
D.xxxxxxxxx(I forgetted)
2. Which is not the javadoc format?
A. @author
B. @parameter
C. @version
D. @return
...

#### T4
Which following option is NOT 10 design UI rule/ practice of XP
#### T5
You are given a piece of what what what code, you want to test the main fuctions, what type of test you need to use?

A. Flaky Testing 

B. Regression Testing 

C. Smoke Testing 

D. Unit Testing



#### T6

.....



## Essay Q1
💛The content of picture about Continuous Deployment.
![](https://codimd.s3.shivering-isles.com/demo/uploads/99175de4-b502-4feb-b097-62e2118d20a4.png)

Maybe the kind of development progress or the name of period will be hidden.


## Essay Q2
💛A pic about Martin's Coupling Metric
![](https://codimd.s3.shivering-isles.com/demo/uploads/ed8fa247-b098-496e-a37c-2ef1279bed3c.png)

1/2. Determing Ca and Ce of Class B.
3. Remember: $$I=Ce/(Ca+Ce)$$

Ca: afferent coupling, Ce: efferent coupling.

## Essay Q3
💛git command/mvn command, and github knowledge 
1. The maven command that clean the project you have builded before and then build again.
2. The file name that Specifies intentionally untracked files to ignore.


## Essay Q4
💛Re-engineering pattern 
Given an **apk** file of a pokemon game **without its source code**. You need to re-engineering it,  which pattern will you choose? 
If you need the source code please explain in which step and why. 
Explain why you choose this pattern.


## Essay 05
💛Cyclomatic complexity 
Given a piece of code, calculate the cyclomatic complexity and explain how you get it.

💙I forget the code, maybe it is the same as following question[Test coding]

## Test Coding
💛Write Junit tests for specified branches and Write Javadoc.
``` java=
   public static boolean isBlank(String cr){
        if(cr==null||cr.length()==0){
            return true;
        }
        for (int a=0;a<cr.length();a++){
            if(Character.isWhitespace(cr.charAt(a))!=false){
                return false;
            }
        }
        return true;
    }
```

1. Write junit tests for line 2 if branch and a test for  line 6 else branch.
2. Write Javadoc for the isBlank method.


## Security
💛Given a piece of news, answer the Asset, Vulnerability, Threat.
🧡Pokemon old version has the full account access automatically. It can access the whole account information including their account on device and network infomation.

Here is a sample website which describes the [threat](http://www.acgwow.com/369080.html).

## Ending
Congrats! You are one step closer to a SE master!