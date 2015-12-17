## Contributing to the project on Github

We will find a project to contribute to, plan a change, fork the project, make the fix and submit a pull request to the project owner.

### Find a project

Browse github.com projects, look at the source. Find a project where you could contribute. If you have no ideas how to contribute it is worth to see a website for the project. Look for "Known issues" part where usually is a list of issues to work on.

In this tutorial I will fork the project [DocEar/PDF-Inspector](https://github.com/Docear/PDF-Inspector). It has a known issue 
> Parameter handling is not the best (if unknown parameters are entered, nothing might work).

### Plan a change

Look at the source code of the project. Identify where the issue is and second think about an approach to resolve the issue.

Browsing the code I found that [OptionParser](https://github.com/Docear/PDF-Inspector/blob/master/src/org/docear/pdf/options/OptionParser.java) uses PosixParser method ```parse``` with an argument ```stopAtNonOption=true```.

```
/**
  * code ommited 
  */

public class OptionParser {	
	private static Configuration config = new Configuration();

	public OptionParser(String[] args) {
		final Parser commandlineparser = new PosixParser();
		final Options options = createCommandLineOptions();
		CommandLine cl = null;
		try {
			cl = commandlineparser.parse(options, args, true); //we will change this line
		}	
```

See [CommandLine.parse ](https://commons.apache.org/proper/commons-cli/apidocs/org/apache/commons/cli/CommandLineParser.html#parse-org.apache.commons.cli.Options-java.lang.String:A-boolean-) 
```
CommandLine parse(Options options,
                  String[] arguments,
                  boolean stopAtNonOption) 
           throws ParseException
```

Now that we have seen where we could apply a change. Let's do it.

### Do changes

- Fork the source code.

Navigate to the repository on github and on the right-top corner press the button "Fork". Once you have a fork clone the repository. Navigate to the fork and copy a clone URL. In your environment use a tool for cloning. I use Netbeans for this. Open "Team" -> "Git" -> "Clone..."
![enter image description here](https://www.filepicker.io/api/file/E3Jqtj6zTtWGQ6F2jSXm "enter image title here")

After pressed "Clone" put a clone URL and press "Next", confirm branch (press "Next") and let Netbeans clone the fork. Once cloning done change the source code. But before we do changes to the source code, we [create a branch](https://help.github.com/articles/creating-and-deleting-branches-within-your-repository/) "stop-at-non-option" to work on. In Netbeans: make sure the project is high lighted in "Projects window" and click "Team"-->"Branch/Tag"-->"Create Branch..", write branch name "stop-at-no-option" and check the box "Checkout created branch" and press "Create".

Once branch created do "Resolve problems" (Netbeans will download for you missing libraries).

- Change the source code.

We will change one line in the source code. The change is

```
cl = commandlineparser.parse(
		options, 
		args   , 
		false   ); // do not stop at non option
```
to view the change diff do in Netbeans "Team"-->"Diff"-->"Diff to HEAD". Here's the textual output of the change.

```
--- HEAD
+++ Modified In Working Tree
@@ -19,7 +19,10 @@
 		final Options options = createCommandLineOptions();
 		CommandLine cl = null;
 		try {
-			cl = commandlineparser.parse(options, args, true);
+			cl = commandlineparser.parse(
+					options, 
+					args   , 
+					false   ); // do not stop at non option
 		}
 		catch (final ParseException exp) {
 			System.err.println("Unexpected exception:" + exp.getMessage());
@@ -146,5 +149,4 @@
 			System.exit(status);
 		}
 	}
-
\ No newline at end of file
 }
```

Commit the changes with a meaningful message "Fixing issue "Parameter handling is not the best (if unknown parameters are entered, nothing might work)". Click "Team"-->"Commit" and write commit message, then press "Commit". Once done, the changes are commited. Push the changes to the remote branch (on github.com).

In Netbeans click "Team"-->"Remote"-->"Push.."-->Next-->check "stop-at-non-option"-->Next-->Finish.

Next pull a change request.

- Pull change request.

Navigate to the branch "stop-at-non-option" on github.com and press "Compare & pull request. Write your message and press "Create pull request".

If you followed and succeeded you just submitted a change request.

Now wait while the owner of the project reviews the change request.

_Questions, comments, critique are welcome! :)_