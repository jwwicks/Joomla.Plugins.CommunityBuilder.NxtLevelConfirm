# Introduction #
<p>This readme assumes you have a Joomla 1.5.x and Community Builder 1.2.x installation already setup. It also assumes you're familiar with editing PHP files. If you're shakey on the latter this process might not be best for you.</p>
<p>This ZIP file contains all the files necessary to setup Community Builder to use a secondary confirmation process to upgrade a user's access level based on a value stored in a CB data field. I personally used it to confirm Purchase ID's for a book.</p>
<p>The plugin uses an email message which is sent to a third party, presumably an Administrator in Joomla. This user then verifies the value submitted and clicks one of the supplied links to confirm or deny the access upgrade.</p>

# Details #
<p>Here's the scenario I wanted to solve:<br>
<ul>
<li>User clicks Login/Register Here button for Community Builder</li>
<li>Registration form displayed with extra NxtLvlWatchField</li>
<li>User fills out form including NxtLvlWatchField value</li>
<li>User submits form</li>
<li>Normal CB User email confirmation sent</li>
<li>User clicks email confirmation link</li>
<li>CB onAfterUserConfirm Event triggers NxtLvlConfirm plugin to email "NxtLvlEmailToUser" to confirm NxtLvlWatchField value, if entered</li>
<li>"NxtLvlEmailToUser" clicks confirmation link which triggers NxtLvlConfirm which upgrades User Access level to NxtLvlOnConfirmedGroup</li>
<li>User able to view/download NxtLvlOnConfirmedGroup content</li>
</ul>
</p>
<p>The process to get this plugin up and running is:<br>
<ol>
<li>Download and install the plugin via the CB Plugin Manager</li>
<li>Add the appropriate fields via the CB Field Manager, one must be named nxtlvlactivation, the other is configurable in the next step</li>
<li>Once the install completes successfully, click the Next Level Confirmation plugin link in the CB Plugin Manager list and configure/enable.</li>
<li>Hack the comprofiler.php core file for CB using the corehack files from svn as a guide.</li>
</ol>
</p>

# Files and Descriptions #
<p>
<ul>
<li>nxtlvlconfirm.xml - Next Level Confirmation CB plugin installation settings file</li>
<li>nxtlvlconfirm.php - Next Level Confirmation CB plugin</li>
<li><a href='http://code.google.com/p/jww-jmla-cb-plg-nxtlvlconfirm/source/browse/trunk/corehack/comprofiler.php'>http://code.google.com/p/jww-jmla-cb-plg-nxtlvlconfirm/source/browse/trunk/corehack/comprofiler.php</a> - CB Core hack example</li>
<li><a href='http://code.google.com/p/jww-jmla-cb-plg-nxtlvlconfirm/source/browse/trunk/corehack/nxtlvlMessages.txt'>http://code.google.com/p/jww-jmla-cb-plg-nxtlvlconfirm/source/browse/trunk/corehack/nxtlvlMessages.txt</a> - Example email settings for the Next Level Confirmation plugin</li>
</ul>
</p>

# To install the CB plugin #
<p>To install the Next Level Confirmation plugin in Community Builder do the following:<br>
<ol>
<li>Download the latest plugin from the <a href='http://code.google.com/p/jww-jmla-cb-plg-nxtlvlconfirm/downloads/list'>downloads page</a> of this project.</li>
<li>Login to the Joomla Administration area for your site and access the Plugin Manager for Community Builder. Click the Install Plugin link, click Browse and select the archive for plugin then click Upload File & Install.</li>
</ol>
</p>

# To create the CB Fields #
<p>Next you'll need to create a couple of extra fields to use during the confirmation process:<br>
<ol>
<li>Go to the CB Field Manager.</li>
<li>Click the New Field button</li>
<li>I use the following as an example for the field settings:<br>
<ul>
<blockquote><li>Type - Text Field</li>
<li>Tab - Contact Info</li>
<li>Name - bookorderid (Note CB will automatically prepend cb<i>to the name)</li></i><li>Title - Book Order ID</li>
<li>Description - This is the Order ID on your invoice</li>
<li>Required - No</li>
<li>Show on Profile - Yes: on 1 line</li>
<li>Display Field Title - Yes</li>
<li>Searchable - No</li>
<li>Read Only - No</li>
<li>Show at Registration - Yes</li>
<li>Published - Yes</li>
<li>Size - 25</li>
<li>Max - 40</li>
<li>Authorized Input - Custom Perl regular expression</li>
<li>Regular Expression - /^[0-9A-z-]{8,35}$/</li>
<li>Error message - The Order ID does not appear to be valid. Should be a single set of characters and numbers at least 8 digits in length.</li>
</ul>
</li>
<li>Save the new Field</li>
<li>Click the New Field button again</li>
<li>I use the following as an example for the field settings:<br>
<ul>
<li>Type - Text Field</li>
<li>Tab - Contact Info</li>
<li>Name - nxtlvlactivation (Note CB will automatically prepend cb<i>to the name)</li></i><li>Title - Next Level Activation Code</li>
<li>Required - No</li>
<li>Show on Profile - No</li>
<li>Display Field Title - No</li>
<li>Searchable - No</li>
<li>Read Only - No</li>
<li>Show at Registration - No</li>
<li>Published - Yes</li>
<li>Size - 50</li>
</ul>
</li>
<li>Save the new Field</li>
</ol>
</p></blockquote>

# To configure the CB plugin #
<p>Next we'll need to configure the settings for the plugin to match the fields we created:<br>
<ol>
<li>Goto the CB Plugin Manager</li>
<li>Click the Next Level Confirmation plugin link</li>
<li>I use the following as an example for the field settings:<br>
<ul>
<blockquote><li>NxtLevel Confirmation Enabled - Yes</li>
<li>NxtLevel Field Name - cb_bookorderid</li>
<li>NxtLevel Group Name - Author (19)</li>
<li>Email From Id - Super Administrator (62)</li>
<li>Email To Id - Super Administrator (62)</li>
<li>All the rest can be left with the defaults. Just be aware if you modify these that the field placeholders need to be there.</li>
</ul>
</li>
</ol>
</p></blockquote>

# To Hack the Core comprofiler.php #
<p>Next we have to hack a core file for CB. Backup Backup Backup. Oh yeah Backup.<br>
<ol>
<li>Copy the CB core comprofiler.php in siteroot/components/com_profiler and store it as a backup.</li>
<li>Locate and download the comprofiler.php from this projects svn</li>
<li>Open a copy of the core comprofiler.php file and add the sections from this projects comprofiler.php that exist between comment blocks /<b>NxtLvl Mod</b>/. There are two sections of code to add. Lines 145 to 155 and 2400 to 2568.</li>
<li>Save/Upload the modified comprofiler.php file to your site.</li>
<li>Test</li>
</ol>
</p>

# Test the Confirmation Process #
<p>
<ol>
<li>Create a test account using the normal CB login process. Confirm the email address by clicking the CB supplied link.</li>
<li>Once confirmed, the test user should receive a confirmation message. The administrative contact you set, should also get a message asking for confirmation of the secondary identifier. Click the confirm link in that email and the test user should be upgraded to the group you specified in the settings.</li>
</ol>
</p>