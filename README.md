# Testing-Assignment-for-Intern-Position---Escrow-Stack
package demo;
	import java.io.File;
	import java.io.IOException;
	import java.time.Duration;
	import java.util.ArrayList;
	import org.openqa.selenium.By;
	import org.openqa.selenium.OutputType;
	import org.openqa.selenium.TakesScreenshot;
	import org.openqa.selenium.WebDriver;
	import org.openqa.selenium.WebElement;
	import org.openqa.selenium.chrome.ChromeDriver;
	import org.openqa.selenium.support.ui.ExpectedConditions;
	import org.openqa.selenium.support.ui.WebDriverWait;
	import com.google.common.io.Files;
public class Escrow_Stack {
	    public static void main(String[] args) throws IOException, InterruptedException {

 
 // Set your chromedriver PATH here
	        System.setProperty("webdriver.edge.driver","./drivers/msedgedriver.exe");

WebDriver driver = new ChromeDriver();
 WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(15));
  driver.manage().window().maximize();

driver.get("https://www.flipkart.com");

	        // Close popup
 try {
	            WebElement closePopup = wait.until(ExpectedConditions.elementToBeClickable(
	                    By.xpath("//button[contains(text(),'✕')]")));
	            closePopup.click();
	        } catch (Exception e) {
	            System.out.println("Login Popup not displayed.");
	        }

	        // Search for Bluetooth Speakers
 WebElement searchBox = driver.findElement(By.name("q"));
	        searchBox.sendKeys("Bluetooth Speakers");
	        searchBox.submit();

	        // Filter → boAt
 wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//div[text()='boAt']"))).click();

	        // Filter → 4★ & above
   wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//div[text()='4★ & above']"))).click();

	        // Sort → Price Low to High
wait.until(ExpectedConditions.elementToBeClickable(By.xpath("//div[text()='Price -- Low to High']"))).click();

Thread.sleep(3000);

	        // Get 1st Product URL
WebElement firstProduct = wait.until(ExpectedConditions.elementToBeClickable(
	                By.xpath("(//a[contains(@class,'CGtC98')])[1]")));

String productURL = firstProduct.getAttribute("href");
System.out.println("Opening product page: " + productURL);

	        // Open Product Page (new navigation)
driver.get(productURL);

Thread.sleep(3000);

	        // Check Available Offers
try {
	            WebElement offersTitle = driver.findElement(By.xpath("//div[text()='Available offers']"));

System.out.println("Offer section is present.");

int offersCount = driver.findElements(By.xpath("//li[contains(@class,'_5VGJZ')]")).size();
	            System.out.println("Number of Offers: " + offersCount);

} catch (Exception e) {
	            System.out.println("Offer section NOT found.");
	        }

	        // SCENARIO 1: Check Add to Cart
 try {
	            WebElement addButton = driver.findElement(By.xpath("//button[contains(text(),'Add to cart')]"));

if (addButton.isDisplayed() && addButton.isEnabled()) {

addButton.click();
	                System.out.println("Add to Cart clicked.");

	                // Wait for cart page
 wait.until(ExpectedConditions.urlContains("cart"));

	                // Screenshot
 takeScreenshot(driver, "cart_result.png");
	                System.out.println("screenshot saved: cart_result.png");

	            }
} catch (Exception e) {

	           // SCENARIO 2 — Product not available
 System.out.println("Product unavailable — could not be added to cart.");

takeScreenshot(driver, "result.png");
	            System.out.println("screenshot saved: result.png");
	        }

 driver.quit();
	    }
public static void takeScreenshot(WebDriver driver, String fileName) throws IOException {
	        File src = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
	        Files.copy(src, new File("./screenshot/.png"));
	    }
	}

	
