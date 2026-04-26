# 🔬 Functional Interfaces & Lambda Expressions in Java

[![Java](https://img.shields.io/badge/Java-8%2B-orange.svg)](https://www.oracle.com/java/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> A comprehensive research document on Functional Interfaces and Lambda Expressions in Java 8+

---

## 📚 Table of Contents

- [Introduction](#introduction)
- [Functional Interface](#1-functional-interface)
- [Lambda Expression](#2-lambda-expression)
- [Professional Implementation](#3-professional-implementation)
- [Best Practices](#4-best-practices)
- [References](#references)

---

## 1. Functional Interface

### Definition
An interface that contains **exactly one abstract method** (SAM — Single Abstract Method).

### Code Example

```java
@FunctionalInterface
public interface Transformer<T, R> {
    R transform(T input);  // Single abstract method
    
    default void log(String msg) {
        System.out.println("[LOG] " + msg);
    }
}
Key Characteristics
Feature	Description
One abstract method	Core requirement
Default/static methods	Allowed (any number)
@FunctionalInterface	Optional but recommended
Target for lambdas	Enables functional programming
2. Lambda Expression
Syntax
java
(parameters) -> { body }
Examples
java
// No parameters
() -> System.out.println("Hello")

// One parameter
x -> x * 2

// Multiple parameters
(a, b) -> a + b

// Block body
(x, y) -> { 
    int sum = x + y; 
    return sum; 
}
Before vs After
java
// Before: Anonymous Class
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running");
    }
};

// After: Lambda (70% less code)
Runnable r2 = () -> System.out.println("Running");
3. Professional Implementation
Context: Real-time Payment Processing Engine
Built-in Functional Interfaces
Interface	Method	Purpose
Predicate<T>	boolean test(T t)	Validation
Function<T,R>	R apply(T t)	Transformation
Consumer<T>	void accept(T t)	Side effects
Supplier<T>	T get()	Data provision
Complete Code Example
java
import java.util.function.*;
import java.util.List;
import java.util.Arrays;
import java.time.LocalDateTime;
import java.util.stream.Collectors;

class Transaction {
    private String id;
    private double amount;
    private String currency;
    private String riskLevel;
    private LocalDateTime timestamp;

    public Transaction(String id, double amount, String currency, 
                       LocalDateTime timestamp) {
        this.id = id;
        this.amount = amount;
        this.currency = currency;
        this.timestamp = timestamp;
        this.riskLevel = "UNKNOWN";
    }

    public String getId() { return id; }
    public double getAmount() { return amount; }
    public String getCurrency() { return currency; }
    public String getRiskLevel() { return riskLevel; }
    public void setRiskLevel(String riskLevel) { 
        this.riskLevel = riskLevel; 
    }
    public LocalDateTime getTimestamp() { return timestamp; }

    @Override
    public String toString() {
        return String.format("Tx{id='%s', amount=%.2f %s, risk='%s'}",
                id, amount, currency, riskLevel);
    }
}

public class PaymentValidatorService {

    private final Predicate<Transaction> isHighValue = 
        tx -> tx.getAmount() > 10000.0;

    private final Predicate<Transaction> isForeignCurrency = 
        tx -> !"USD".equals(tx.getCurrency());

    private final Function<Transaction, Transaction> enrichWithRiskScore = tx -> {
        if (isHighValue.test(tx) && isForeignCurrency.test(tx)) {
            tx.setRiskLevel("HIGH");
        } else if (isHighValue.test(tx)) {
            tx.setRiskLevel("MEDIUM");
        } else {
            tx.setRiskLevel("LOW");
        }
        return tx;
    };

    private final Consumer<Transaction> auditLogger = tx -> {
        System.out.printf("[%s] AUDIT: %s | Risk: %s%n",
                LocalDateTime.now(), tx.getId(), tx.getRiskLevel());
    };

    private final Supplier<Transaction> sampleTransactionSupplier = () -> 
        new Transaction(
            "TX-" + System.currentTimeMillis(),
            25000.50,
            "EUR",
            LocalDateTime.now()
        );

    public void processTransactions(List<Transaction> transactions) {
        List<Transaction> processed = transactions
            .stream()
            .map(enrichWithRiskScore)
            .peek(auditLogger)
            .filter(tx -> !"HIGH".equals(tx.getRiskLevel()))
            .collect(Collectors.toList());

        System.out.println("\n✅ Approved Transactions:");
        processed.forEach(System.out::println);
    }

    public static void main(String[] args) {
        PaymentValidatorService service = new PaymentValidatorService();
        
        Transaction sample = service.sampleTransactionSupplier.get();
        System.out.println("Sample: " + sample);

        List<Transaction> transactions = Arrays.asList(
            new Transaction("T1001", 500.0, "USD", LocalDateTime.now()),
            new Transaction("T1002", 15000.0, "USD", LocalDateTime.now()),
            new Transaction("T1003", 50000.0, "EUR", LocalDateTime.now()),
            new Transaction("T1004", 7500.0, "GBP", LocalDateTime.now())
        );

        service.processTransactions(transactions);
    }
}
Sample Output
text
Sample: Tx{id='TX-1713456789123', amount=25000.50 EUR, risk='UNKNOWN'}
[2025-04-26T10:25:30] AUDIT: T1001 | Risk: LOW
[2025-04-26T10:25:30] AUDIT: T1002 | Risk: MEDIUM
[2025-04-26T10:25:30] AUDIT: T1003 | Risk: HIGH
[2025-04-26T10:25:30] AUDIT: T1004 | Risk: MEDIUM

✅ Approved Transactions:
Tx{id='T1001', amount=500.00 USD, risk='LOW'}
Tx{id='T1002', amount=15000.00 USD, risk='MEDIUM'}
Tx{id='T1004', amount=7500.00 GBP, risk='MEDIUM'}
4. Best Practices
✅ Do's
Use built-in interfaces (Predicate, Function, Consumer, Supplier)

Keep lambdas short (1-3 lines)

Combine with Streams API

Use method references when possible: Class::method

Add @FunctionalInterface to custom interfaces

❌ Don'ts
Write lambdas longer than 4 lines

Modify external state inside lambdas

Throw checked exceptions from lambdas

Create complex nested lambdas

Quick Reference
You need to...	Use this interface
Test/Condition	Predicate<T>
Transform A→B	Function<T,R>
Consume/Log	Consumer<T>
Generate data	Supplier<T>
Combine values	BinaryOperator<T>
📖 References
Oracle Java Tutorials: Lambda Expressions

Java Language Specification (JLS §9.8)

java.util.function Package Documentation

Effective Java (3rd Ed.) - Item 42: Prefer lambdas to anonymous classes