**<h1> Selenium | STANDARDmade</h1>**

## <span style="color:#555555"><u> **OVERVIEW** </u></span>
[`Selenium Test`] by STANDARDmade, LLC
- Find Bacon
  - Automation Stuff
  - Selenium Stuff
  - Google Stuff
  - Java Stuff
  - :bacon: Stuff


## <span style="color:#555555"><u> **POINTS OF CONTACT** </u></span>
If any issues arise for any of the below mentioned areas, please draft a strongly worded email and never send it to: **kitt@made.llc** 



## <span style="color:#555555"><u> **CORE SOLUTIONS** </u></span>
| Stack  | Versions |
| ------------- |:-------------:|
| Java | 17 |
| Eclipse IDE | 16.15.0 |
| Selenium Jar | 4.7.0 |
| Chrome Driver | 108.0.5359.71 |

1. Java 17 - https://www.oracle.com/java/technologies/javase-jdk16-downloads.html

2. Eclipse IDE - https://www.eclipse.org/downloads/

3. Selenium Jar - https://www.selenium.dev/downloads/ (get latest stable version)
	a. in Eclipse, right click on project foler and select Build Path -> Configure Build Path
	b. select the Libraries tab and select Class Path
	c. select option to Add External JARs and select the previously downloaded Selenium jar file and import

4. Chrome Driver - https://sites.google.com/a/chromium.org/chromedriver/ (select version that is not higher than your local version of Chrome)

## <span style="color:#555555"><u> **CORE DEVELPOMENT** </u></span>
**Installation guide to deploy, run, and find | :bacon::bacon: | using Selenium and GoogleDriver by STANDARDmade, LLC.** 

You can run this in 2 ways - both methods require that you place the attached imperfect.jar in the following directory > <span style="color:green">C:\AUTOMATION\WORKSPACE\IMPERFECT</span> Both methods also display the results in the cmd terminal window - so do not close

**METHOD 1)** from your OS terminal (type cmd into your Windows search bar to open the command line interface) type the commands below. 
This will change the current directory to point to where the jar file is located and run it using the java -jar command. Test results are written to the CLI during execution:
- `java -version`
			use this to verify you have java 17 installed (not sure if code compiled with jdk-17 is backwards compatable with lower version of java, so this is suggested)
- `cd <folder path containing jar file>`
- `java -jar imperfect.jar`
	
**METHOD 2)** Double click the attached <span style="color:hotpink">RUN_ME-imperfect-foods.bat</span> and enjoy!
	
NOTE: I did not close the browser after the test run so you can verify the Google page results. Test results are shown in the CLI.




## <span style="color:#555555"><u> **CODE** </u></span>
Feel free to check my code below...

<details>
  <summary><span style="color:mediumpurple"> CLICK TO EXPAND </span></summary>

``` java
// FIND THE BACON [java]
public class ImperfectClass {

	public static void main(String[] args) throws InterruptedException {

		// point to chrome driver and define
		System.setProperty("webdriver.chrome.driver", "C:\\BrowserDrivers\\chromedriver.exe");
		WebDriver driver = new ChromeDriver();

		// # 1
		// launch browser, maximize, delete cookies, and add wait timers
		driver.get("http://www.google.com");
		driver.manage().window().maximize();
		driver.manage().deleteAllCookies();
		driver.manage().timeouts().pageLoadTimeout(40, TimeUnit.SECONDS);
		driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);

		// find search element and enter keyword
		WebElement search = driver.findElement(By.name("q"));
		search.sendKeys("bacon");

		// create list from google suggested search xpath and count listed items
		List<WebElement> list = driver
				.findElements(By.xpath("//ul[@role='listbox']//li//descendant::div[@class='sbl1']"));
		System.out.println("There are " + list.size() + " suggested keyword searches");

		// # 2
		// find 'Bacon' in suggested search list and select
		for (int i = 0; i < list.size(); i++) {
			String listitem = list.get(i).getText();
			System.out.println("Searching for plain ole '" + listitem + "'");

			if (listitem.contentEquals("Bacon")) {
				list.get(i).click();
				break;
			}
		}

		// # 3,
		// find first search result in list (always <h3> in Google)
		WebElement topBacon = driver.findElement(By.tagName("h3"));
		String topBaconHeader = topBacon.getText();
		if (topBaconHeader.contentEquals("Bacon - Wikipedia")) {
			System.out.println("GOOD: '" + topBaconHeader + "' = top 'Bacon' result");
		}
		// BONUS
		// verify Sir Francis Bacon is not the first 'Bacon' result
		else if (topBaconHeader.contentEquals("Francis Bacon - Wikipedia")) {
			System.out.println("BAD: '" + topBaconHeader + "' = top 'Bacon' result");
		}

		// # 4
		// verify nutrition facts info exists - throws exception if not found
		WebDriverWait waiter = new WebDriverWait(driver, 5000);
		waiter.until(
				ExpectedConditions.presenceOfElementLocated(By.xpath("//div[contains(text(),'Nutrition Facts')]")));
		WebElement nutrition = driver.findElement(By.xpath("//div[contains(text(),'Nutrition Facts')]"));
		String nutritionInfo = nutrition.getText();
		System.out.println("GOOD: " + nutritionInfo + " found");

		// # 5
		// verify nutrition facts dropdown exists - throws exception if not found
		waiter.until(ExpectedConditions.presenceOfElementLocated(By.xpath("//select[@class='Ev4pye kno-nf-fs']")));
		WebElement nutritionDropDown = driver.findElement(By.xpath("//select[@class='Ev4pye kno-nf-fs']"));

		// # 6
		// select 'Bacon, baked' from dropdown options
		Select sel = new Select(nutritionDropDown);
		sel.selectByVisibleText("Bacon, baked");

		// # 7
		// confirm calories = 44
		List<WebElement> calories = driver
				.findElements(By.xpath("//tr[@class='PZPZlf kno-nf-cq']/td/span[@class='abs']"));
		for (int j = 0; j < calories.size(); j++) {
			String calorieInfo = calories.get(j).getText();

			if (calorieInfo.contentEquals("44")) {
				System.out.println("GOOD: " + calorieInfo + " calories found");
				break;
			} else {
				System.out.println("BAD: " + calorieInfo + " calories found");
			}
		}

		// close
		driver.quit();
	}
}
```

</details>