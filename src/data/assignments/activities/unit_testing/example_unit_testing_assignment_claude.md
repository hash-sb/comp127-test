# Unit Testing Assignment: Understanding the Power and Limitations

## Assignment Overview

**Course:** Introduction to Computer Science (Java)  
**Topic:** Unit Testing Concepts and Limitations  
**Duration:** 2 weeks  
**Points:** 100 points

## Learning Objectives

By completing this assignment, you will:
- Understand the fundamental purpose and benefits of unit testing
- Recognize the limitations and blind spots of unit testing
- Analyze existing test suites for completeness and effectiveness
- Identify scenarios where high code coverage doesn't guarantee bug-free code
- Develop critical thinking skills about software quality assurance

## Assignment Purpose

Unit testing is a cornerstone of software development, but it's not a silver bullet. This assignment will help you understand both what unit testing can accomplish and where it falls short. You'll learn that while tests are invaluable tools, they require thoughtful design and cannot replace careful reasoning about your code.

**Important Note:** This assignment focuses on testing concepts, not Java syntax or JUnit specifics. You may write pseudocode or simplified Java-like code for your examples.

## Part 1: Conceptual Understanding (25 points)

### Question 1.1 (10 points)
Explain in your own words what unit testing is and why software developers use it. Give three specific benefits and one major limitation.

### Question 1.2 (15 points)
Consider this simple method:

```
public static int divide(int a, int b) {
    return a / b;
}
```

A student wrote this test:

```
@Test
public void testDivide() {
    assertEquals(4, divide(8, 2));
    assertEquals(1, divide(5, 5));
}
```

**a)** What scenarios does this test miss? List at least 3 specific cases.  
**b)** Why might the student think their test is sufficient?  
**c)** What could go wrong in production with this level of testing?

## Part 2: Analyzing Incomplete Test Coverage (25 points)

### Question 2.1 (15 points)
Examine this password validation method:

```
public static boolean isValidPassword(String password) {
    if (password == null || password.length() < 8) {
        return false;
    }
    
    boolean hasUppercase = false;
    boolean hasLowercase = false;
    boolean hasDigit = false;
    
    for (int i = 0; i < password.length(); i++) {
        char c = password.charAt(i);
        if (Character.isUpperCase(c)) hasUppercase = true;
        if (Character.isLowerCase(c)) hasLowercase = true;
        if (Character.isDigit(c)) hasDigit = true;
    }
    
    return hasUppercase && hasLowercase && hasDigit;
}
```

Here are the existing tests:

```
@Test
public void testValidPasswords() {
    assertTrue(isValidPassword("Password123"));
    assertTrue(isValidPassword("MySecret9"));
}

@Test
public void testInvalidPasswords() {
    assertFalse(isValidPassword("password"));  // no uppercase or digit
    assertFalse(isValidPassword("SHORT1"));    // too short
}
```

**a)** Calculate the approximate line coverage percentage of these tests.  
**b)** List 3 important test cases that are missing.  
**c)** Even if we achieved 100% line coverage, explain why the code could still have bugs.

### Question 2.2 (10 points)
A code coverage tool reports 95% line coverage on a project. The development team celebrates, saying "We're almost perfect!"

Explain why this celebration might be premature. Give a specific example of how code with 95% coverage could still have serious bugs.

## Part 3: The Illusion of Complete Testing (30 points)

### Question 3.1 (20 points)
Consider this shopping cart discount calculator:

```
public static double calculateDiscount(double cartTotal, String customerType, int loyaltyYears) {
    double discount = 0.0;
    
    if (customerType.equals("PREMIUM")) {
        discount = 0.15;
    } else if (customerType.equals("REGULAR")) {
        discount = 0.05;
    }
    
    if (loyaltyYears > 5) {
        discount += 0.10;
    } else if (loyaltyYears > 2) {
        discount += 0.05;
    }
    
    return Math.min(discount, 0.30); // Cap at 30%
}
```

A comprehensive test suite covers:
- All customer types (PREMIUM, REGULAR, null, invalid strings)
- All loyalty year ranges (0, 1, 3, 6, 10 years)
- Edge cases for the 30% cap
- **Result: 100% line coverage, 100% branch coverage**

**a)** Despite perfect coverage, identify 2 realistic bugs this code might have.  
**b)** Write a specific test case that would pass with the current implementation but reveals a logical error.  
**c)** Explain why achieving 100% coverage doesn't guarantee the method works correctly for all business requirements.

### Question 3.2 (10 points)
A developer says: "If all my unit tests pass, I know my code is correct."

Write a brief response (3-4 sentences) explaining why this statement is problematic. Include one concrete example scenario.

## Part 4: Writing Tests and Finding Limitations (20 points)

### Question 4.1 (15 points)
Write unit tests for this email validation method. Then identify the limitations of your own tests.

```
public static boolean isValidEmail(String email) {
    if (email == null || !email.contains("@")) {
        return false;
    }
    
    int atIndex = email.indexOf("@");
    if (atIndex == 0 || atIndex == email.length() - 1) {
        return false;
    }
    
    return email.indexOf("@", atIndex + 1) == -1; // Only one @ symbol
}
```

**a)** Write 4-6 test cases in pseudocode or simplified Java syntax.  
**b)** List 2 scenarios where your tests might pass but the email validation is still incorrect.  
**c)** Explain what additional testing approaches (beyond unit tests) might catch these issues.

### Question 4.2 (5 points)
Give one example of a type of bug that unit testing is particularly bad at catching, and briefly explain why.

## Assessment Criteria

Your assignment will be evaluated on:

### Conceptual Understanding (40%)
- Accurate explanation of unit testing principles
- Recognition of testing limitations
- Understanding of the relationship between coverage and quality

### Analysis Skills (35%)
- Ability to identify missing test cases
- Recognition of logical errors despite passing tests
- Understanding of why coverage metrics can be misleading

### Critical Thinking (20%)
- Thoughtful evaluation of testing strategies
- Recognition of real-world complexities
- Understanding of testing as part of broader quality assurance

### Communication (5%)
- Clear, concise explanations
- Proper use of technical terminology
- Well-organized responses

## Submission Guidelines

- Submit your responses in a single document (PDF or Word)
- Use clear headings for each question
- For code examples, use readable formatting
- Aim for concise but complete answers
- Total length should be 4-6 pages

## Academic Integrity

This is an individual assignment. While you may discuss general concepts with classmates, all written work must be your own. Copying test cases or explanations from online sources without attribution constitutes plagiarism.

---

**Due Date:** [Insert appropriate date]  
**Late Policy:** [Insert course policy]  
**Questions?** Contact the instructor during office hours or via email.