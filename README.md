package com.assignment;

import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.annotations.*;
import org.testng.asserts.SoftAssert;
import java.time.Duration;

public class TestScenarios {
    WebDriver driver;

    @BeforeMethod
    @Parameters({"browser", "url"})
    public void setup(String browser, String url) {
        if (browser.equalsIgnoreCase("chrome")) {
            driver = new ChromeDriver();
        } else if (browser.equalsIgnoreCase("firefox")) {
            driver = new FirefoxDriver();
        }
        driver.manage().window().maximize();
        driver.get(url);
    }

    @Test
    public void testScenario1() {
        SoftAssert softAssert = new SoftAssert();

        // 1. Explicit wait for DOM to be fully loaded
        new WebDriverWait(driver, Duration.ofSeconds(10)).until(
            webDriver -> ((JavascriptExecutor) webDriver)
                .executeScript("return document.readyState").equals("complete")
        );

        // 2. SoftAssert for Page Title (Expected to fail)
        String actualTitle = driver.getTitle();
        softAssert.assertEquals(actualTitle, "TestMu AI", "Title mismatch found!");

        // 3. Proceeding to following statement after expected failure
        System.out.println("Proceeding with the test execution after soft assertion...");
        
        softAssert.assertAll();
    }

    @AfterMethod
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
