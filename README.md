<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>


Installing 

<p>Installing <strong>osTicket</strong> on <strong>Microsoft Azure</strong> using <strong>Virtual Machines (Compute), Remote Desktop, and IIS</strong> involves multiple steps. Here's a detailed guide:</p>
<hr />
<h2><strong>Step 1: Create a Virtual Machine in Microsoft Azure</strong></h2>
<h3><strong>1.1 Create a Windows Server Virtual Machine</strong></h3>
<ol>
<li>
<p><strong>Log in to Azure Portal</strong>: <a href="https://portal.azure.com/" target="_new" rel="noopener">Azure Portal</a></p>
</li>
<li>
<p><strong>Create a Virtual Machine</strong>:</p>
<ul>
<li>Go to <strong>Azure Virtual Machines</strong>.</li>
<li>Click <strong>"Create"</strong> &rarr; <strong>"Azure Virtual Machine"</strong>.</li>
<li>Choose a <strong>Windows Server 2019 or 2022</strong> image.</li>
<li>Select an appropriate <strong>VM size</strong> (at least <strong>B2ms</strong> for small setups).</li>
<li>Set <strong>Administrator username and password</strong>.</li>
<li>Allow <strong>RDP (3389) and HTTP (80) ports</strong> in networking settings.</li>
<li>Click <strong>Create</strong> and wait for the deployment to finish.</li>
</ul>
</li>
<li>
<p><strong>Connect to the VM</strong>:</p>
<ul>
<li>Go to your Virtual Machine in Azure.</li>
<li>Click <strong>"Connect" &rarr; "RDP"</strong>.</li>
<li>Download and open the <strong>RDP file</strong> to connect remotely.</li>
</ul>
</li>
</ol>
<hr />
<h2><strong>Step 2: Install IIS (Internet Information Services)</strong></h2>
<ol>
<li><strong>Open Server Manager</strong>.</li>
<li>Click <strong>"Manage" &rarr; "Add Roles and Features"</strong>.</li>
<li>Choose <strong>"Role-based or feature-based installation"</strong>.</li>
<li>Select your VM's server and click <strong>Next</strong>.</li>
<li>Under <strong>Server Roles</strong>, select:
<ul>
<li><strong>Web Server (IIS)</strong></li>
<li>Under "Application Development" select:
<ul>
<li><strong>CGI</strong></li>
<li><strong>ISAPI Extensions</strong></li>
<li><strong>ISAPI Filters</strong></li>
</ul>
</li>
<li>Under "Security" select:
<ul>
<li><strong>Request Filtering</strong></li>
<li><strong>Windows Authentication</strong></li>
</ul>
</li>
</ul>
</li>
<li>Click <strong>Next &rarr; Install</strong>.</li>
<li>After installation, restart your VM.</li>
</ol>
<hr />
<h2><strong>Step 3: Install PHP and MySQL (MariaDB)</strong></h2>
<h3><strong>3.1 Install PHP</strong></h3>
<ol>
<li>Download <strong>PHP 8.x</strong> (Non-Thread Safe version) from <a href="https://windows.php.net/download/" target="_new" rel="noopener">Windows PHP Download</a>.</li>
<li>Extract PHP to <code>C:\PHP</code>.</li>
<li>Rename <code>php.ini-development</code> to <code>php.ini</code>.</li>
<li>Edit <code>php.ini</code>:
<ul>
<li>Enable required extensions by removing <code>;</code> from:
<div>
<div>ini</div>
<div>
<div>
<div><span data-state="closed"><button>Copier</button></span><span data-state="closed"><button>Modifier</button></span></div>
</div>
</div>
<div dir="ltr"><code>extension=mysqli extension=mbstring extension=curl extension=gd </code></div>
</div>
</li>
<li>Set <code>cgi.fix_pathinfo=1</code></li>
</ul>
</li>
<li>Add <strong>PHP to the system path</strong>:
<ul>
<li>Go to <strong>System Properties &rarr; Environment Variables</strong>.</li>
<li>Add <code>C:\PHP</code> to the <strong>Path</strong> variable.</li>
</ul>
</li>
</ol>
<h3><strong>3.2 Install MySQL (MariaDB)</strong></h3>
<ol>
<li>Download <strong>MariaDB</strong> from <a href="https://mariadb.org/download/" target="_new" rel="noopener">MariaDB Website</a>.</li>
<li>Install using default settings.</li>
<li>During setup:
<ul>
<li>Set the root password.</li>
<li>Enable <strong>MySQL as a service</strong>.</li>
</ul>
</li>
<li>After installation, open <strong>MySQL Command Line</strong> and create a database:
<div>
<div>sql</div>
<div>
<div>
<div><span data-state="closed"><button>Copier</button></span><span data-state="closed"><button>Modifier</button></span></div>
</div>
</div>
<div dir="ltr"><code>CREATE DATABASE osticket; CREATEUSER'ostuser'@'localhost' IDENTIFIED BY'password'; GRANTALL PRIVILEGES ON osticket.*TO'ostuser'@'localhost'; FLUSH PRIVILEGES; </code></div>
</div>
</li>
</ol>
<hr />
<h2><strong>Step 4: Download and Configure osTicket</strong></h2>
<h3><strong>4.1 Download osTicket</strong></h3>
<ol>
<li>Download the latest osTicket from <a href="https://osticket.com/download/" target="_new" rel="noopener">osTicket.com</a>.</li>
<li>Extract the files to <code>C:\inetpub\wwwroot\osticket</code>.</li>
</ol>
<h3><strong>4.2 Configure IIS for osTicket</strong></h3>
<ol>
<li>Open <strong>IIS Manager</strong> (<code>inetmgr</code>).</li>
<li>Under <strong>Sites</strong>, select <strong>Default Web Site</strong>.</li>
<li>Click <strong>"Add Application"</strong>:
<ul>
<li><strong>Alias</strong>: <code>osticket</code></li>
<li><strong>Physical Path</strong>: <code>C:\inetpub\wwwroot\osticket</code></li>
<li><strong>Application Pool</strong>: Ensure it uses <strong>PHP</strong>.</li>
</ul>
</li>
<li>Set permissions:
<ul>
<li>Right-click <code>C:\inetpub\wwwroot\osticket</code>, go to <strong>Properties &rarr; Security</strong>.</li>
<li>Add <code>IUSR</code> and <code>IIS_IUSRS</code> with <strong>Modify</strong> permissions.</li>
</ul>
</li>
</ol>
<h3><strong>4.3 Set IIS Handler Mappings</strong></h3>
<ol>
<li>In IIS Manager, go to <strong>Handler Mappings</strong>.</li>
<li>Add a new <strong>FastCGI Handler</strong> for PHP:
<ul>
<li><strong>Request path</strong>: <code>*.php</code></li>
<li><strong>Executable</strong>: <code>C:\PHP\php-cgi.exe</code></li>
<li>Click <strong>OK</strong>.</li>
</ul>
</li>
</ol>
<hr />
<h2><strong>Step 5: Install osTicket</strong></h2>
<ol>
<li>Open a browser and go to:
<div>
<div>arduino</div>
<div>
<div>
<div><span data-state="closed"><button>Copier</button></span><span data-state="closed"><button>Modifier</button></span></div>
</div>
</div>
<div dir="ltr"><code>http://&lt;your-azure-vm-public-ip&gt;/osticket</code></div>
</div>
</li>
<li>Follow the osTicket setup wizard:
<ul>
<li>Enter the <strong>MySQL Database</strong> details:
<ul>
<li>Database: <code>osticket</code></li>
<li>User: <code>ostuser</code></li>
<li>Password: <code>&lt;your password&gt;</code></li>
</ul>
</li>
<li>Set up the <strong>Administrator Account</strong>.</li>
</ul>
</li>
<li>Click <strong>Install Now</strong> and wait for installation to complete.</li>
</ol>
<hr />
<h2><strong>Step 6: Secure the Installation</strong></h2>
<ol>
<li><strong>Delete the setup directory</strong>:
<div>
<div>sh</div>
<div>
<div>
<div><span data-state="closed"><button>Copier</button></span><span data-state="closed"><button>Modifier</button></span></div>
</div>
</div>
<div dir="ltr"><code>rd /s /q C:\inetpub\wwwroot\osticket\setup </code></div>
</div>
</li>
<li>Restart IIS:
<div>
<div>nginx</div>
<div>
<div>
<div><span data-state="closed"><button>Copier</button></span><span data-state="closed"><button>Modifier</button></span></div>
</div>
</div>
<div dir="ltr"><code>iisreset</code></div>
</div>
</li>
</ol>
<hr />
<h2><strong>Step 7: Access osTicket</strong></h2>
<ul>
<li>Admin Panel:
<div>
<div>perl</div>
<div>
<div>
<div><span data-state="closed"><button>Copier</button></span><span data-state="closed"><button>Modifier</button></span></div>
</div>
</div>
<div dir="ltr"><code>http://&lt;your-azure-vm-public-ip&gt;/osticket/scp </code></div>
</div>
</li>
<li>Staff Login:
<div>
<div>arduino</div>
<div>
<div>
<div><span data-state="closed"><button>Copier</button></span><span data-state="closed"><button>Modifier</button></span></div>
</div>
</div>
<div dir="ltr"><code>http://&lt;your-azure-vm-public-ip&gt;/osticket</code></div>
</div>
</li>
</ul>
<hr />
<h3><strong>Final Notes</strong></h3>
<ul>
<li>Consider setting up <strong>SSL</strong> with <strong>Let's Encrypt</strong> or Azure's built-in SSL options.</li>
<li>Enable automatic <strong>backups</strong> for the database in Azure.</li>
<li>Optimize performance by using <strong>Azure Load Balancer</strong> if you scale to multiple VMs.</li>
</ul>
<br />
