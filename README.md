# How-to-install-MySQL-in-WSL








        <div class="relative flex flex-col grow">
            <main id=main-content class=grow>
                <article>
                    <header class=max-w-prose>
                        <h1 class="mt-0 text-4xl font-extrabold text-neutral-900 dark:text-neutral">How to install
                            MySQL on WSL 2 (Ubuntu)</h1>
                        <div class="mt-8 mb-12 text-base text-neutral-500 dark:text-neutral-400 print:hidden">
                            <div class="flex flex-row flex-wrap items-center"><time
                                    datetime="2021-08-08 16:45:48 +0000 UTC">8 August 2021</time><span
                                    class="px-2 text-primary-500">&#183;</span><span title="Reading time">7
                                    mins</span></div>
                        </div>
                    </header>
                    <section class="flex flex-col max-w-full mt-0 prose dark:prose-invert lg:flex-row">
                        <div class="order-first px-0 lg:order-last lg:max-w-xs ltr:lg:pl-8 rtl:lg:pr-8">
                            <div class="toc ltr:pl-5 rtl:pr-5 print:hidden lg:sticky lg:top-10">
                                <details open
                                    class="mt-0 overflow-hidden rounded-lg ltr:-ml-5 ltr:pl-5 rtl:-mr-5 rtl:pr-5">
                                    <summary
                                        class="block py-1 text-lg font-semibold cursor-pointer bg-neutral-100 text-neutral-800 ltr:-ml-5 ltr:pl-5 rtl:-mr-5 rtl:pr-5 dark:bg-neutral-700 dark:text-neutral-100 lg:hidden">
                                        Table of Contents</summary>
                                    <div
                                        class="py-2 border-dotted border-neutral-300 ltr:-ml-5 ltr:border-l ltr:pl-5 rtl:-mr-5 rtl:border-r rtl:pr-5 dark:border-neutral-600">
                                        <nav id=TableOfContents>
                                            <ul>
                                                <li><a href=#update-wsl-2-ubuntu>Update WSL 2 Ubuntu</a></li>
                                                <li><a href=#install-mysql-server>Install MySQL server</a></li>
                                                <li><a href=#confirm-installation>Confirm installation</a></li>
                                                <li><a href=#secure-the-mysql-server>Secure the MySQL server</a>
                                                </li>
                                                <li><a href=#verify-mysql-is-running>Verify MySQL is running</a>
                                                </li>
                                                <li><a href=#allow-remote-access>Allow remote access</a></li>
                                                <li><a href=#configure-heidisql>Configure HeidiSQL</a></li>
                                                <li><a href=#change-the-default-port>Change the default port</a>
                                                </li>
                                                <li><a href=#start-mysql-automatically-when-wsl-starts>Start MySQL
                                                        automatically when WSL starts</a></li>
                                                <li><a href=#manually-start-mysql>Manually start MySQL</a></li>
                                                <li><a href=#troubleshooting>Troubleshooting</a>
                                                    <ul>
                                                        <li><a href=#typical-errors-and-possible-reasons>Typical
                                                                errors and possible reasons</a></li>
                                                        <li><a href=#basic-troubleshooting>Basic troubleshooting</a>
                                                        </li>
                                                        <li><a href=#check-the-server-is-running>Check the server is
                                                                running</a></li>
                                                        <li><a href=#check-the-mysql-server-for-errors>Check the
                                                                MySQL server for errors</a></li>
                                                        <li><a href=#check-the-port>Check the port</a></li>
                                                    </ul>
                                                </li>
                                                <li><a href=#stop-mysql-and-wsl>Stop MySQL and WSL</a></li>
                                            </ul>
                                        </nav>
                                    </div>
                                </details>
                            </div>
                        </div>
                        <div class="min-w-0 min-h-0 max-w-prose grow">
                            <p>
                            <figure><img class="my-0 rounded-md"
                                    srcset="/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/tobias-fischer-PkbZahEG2Ng-unsplash_hu9647567714aa6ed534a27308e14d31e2_1863143_330x0_resize_q75_box.jpg 330w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/tobias-fischer-PkbZahEG2Ng-unsplash_hu9647567714aa6ed534a27308e14d31e2_1863143_660x0_resize_q75_box.jpg 660w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/tobias-fischer-PkbZahEG2Ng-unsplash_hu9647567714aa6ed534a27308e14d31e2_1863143_1024x0_resize_q75_box.jpg 1024w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/tobias-fischer-PkbZahEG2Ng-unsplash_hu9647567714aa6ed534a27308e14d31e2_1863143_1320x0_resize_q75_box.jpg 2x"
                                    src=/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/tobias-fischer-PkbZahEG2Ng-unsplash_hu9647567714aa6ed534a27308e14d31e2_1863143_660x0_resize_q75_box.jpg
                                    alt="person sitting on couch inside building">
                                <figcaption>Person sitting on couch inside building</figcaption>
                            </figure>Photo by <a
                                href="https://unsplash.com/@tofi?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText"
                                target=_blank>Tobias Fischer</a>
                            on <a
                                href="https://unsplash.com/s/photos/database?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText"
                                target=_blank>Unsplash</a></p>
                            <p>How to install and configure MySQL 8.0 on WSL 2 (Ubuntu), this guide
                                assumes <a href=https://docs.microsoft.com/en-us/windows/wsl/install-win10
                                    target=_blank>WSL 2 is already installed with Ubuntu</a>.</p>
                            <p>I normally use <a href=https://laragon.org/ target=_blank>Laragon</a> for my local
                                development, for most sites this works well.However, for
                                development of Silverstripe CMS projects I regularly have problems exporting my
                                MySQL local MySQL database from Windows
                                and importing to Linux MySQL. This is due to Silverstripe using Pascal case table
                                names. Normally the Silverstripe CMS,
                                on the Linux server will rename the lower case table names generated on Windows
                                MySQL, however the Fluent plugin does
                                not support this feature. I therefore have to manually rename the lower case table
                                names for Fluent to Pascal case. By
                                using MySQL on WSL 2 I get around this problem. I still get the benefit of Laragon
                                for Apache, PHP, Node, HeidiSQL etc.</p>
                            <h2 id=update-wsl-2-ubuntu class="relative group">Update WSL 2 Ubuntu <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#update-wsl-2-ubuntu
                                        aria-label=Anchor>#</a></span></h2>
                            <p>-
                                Source: <a href=https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database
                                    target=_blank>Get started with databases on Windows Subsystem for Linux</a></p>
                            <p>Open your WSL terminal (ie. Ubuntu).</p>
                            <p>Fetch the latest updates and upgrade</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>sudo apt update <span class=o>&amp;&amp;</span> sudo apt upgrade
    </span></span></code></pre>
                            </div>
                            <p>Enter the password you set when you installed Ubuntu WSL.</p>
                            <p>Wait for the packages to install, there will be a lot of information scrolling up the
                                screen!</p>
                            <h2 id=install-mysql-server class="relative group">Install MySQL server <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#install-mysql-server
                                        aria-label=Anchor>#</a></span></h2>
                            <p>Once the packages have updated, install MySQL server:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>sudo apt install mysql-server
    </span></span></code></pre>
                            </div>
                            <p>Wait for the server to install, again there will be a lot of information scrolling up
                                the screen!</p>
                            <h2 id=confirm-installation class="relative group">Confirm installation <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#confirm-installation
                                        aria-label=Anchor>#</a></span></h2>
                            <p>Installation can be confirmed by checking the version number:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>mysql --version
    </span></span></code></pre>
                            </div>
                            <p>You should see output like:</p>
                            <blockquote>
                                <p>mysql Ver 8.0.26-0ubuntu0.20.04.2 for Linux on x86_64 ((Ubuntu))</p>
                            </blockquote>
                            <h2 id=secure-the-mysql-server class="relative group">Secure the MySQL server <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#secure-the-mysql-server
                                        aria-label=Anchor>#</a></span></h2>
                            <p>You may also want to run the included security script. This changes some less secure
                                default options for things like
                                remote root logins and sample users.</p>
                            <p>First start the MySQL server:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>sudo /etc/init.d/mysql start
    </span></span></code></pre>
                            </div>
                            <p>Then run the security script:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>sudo mysql_secure_installation
    </span></span></code></pre>
                            </div>
                            <blockquote>
                                <p>Securing the MySQL server deployment.</p>
                                <p>Connecting to MySQL using a blank password.</p>
                                <p>VALIDATE PASSWORD COMPONENT can be used to test passwords and improve security.
                                    It checks the strength of password and
                                    allows the users to set only those passwords which are secure enough. Would you
                                    like to setup VALIDATE PASSWORD
                                    component?</p>
                                <p>Press y|Y for Yes, any other key for No: <strong>n</strong></p>
                                <p>Please set the password for root here.</p>
                                <p>New password: <strong>root</strong></p>
                                <p>Re-enter new password: <strong>root</strong></p>
                                <p>By default, a MySQL installation has an anonymous user, allowing anyone to log
                                    into MySQL without having to have a
                                    user account created for them. This is intended only for testing, and to make
                                    the installation go a bit smoother. You
                                    should remove them before moving into a production environment.</p>
                                <p>Remove anonymous users? (Press y|Y for Yes, any other key for No) :
                                    <strong>y</strong>
                                </p>
                                <p>Success.</p>
                                <p>Note: Normally, root should only be allowed to connect from
                                    &rsquo;localhost&rsquo;. This ensures that someone cannot guess at the root
                                    password from the network.</p>
                                <p>Disallow root login remotely? (Press y|Y for Yes, any other key for No) :
                                    <strong>n</strong>
                                </p>
                                <p>&mldr; skipping.</p>
                                <p>By default, MySQL comes with a database named &rsquo;test&rsquo; that anyone can
                                    access. This is also intended only for testing,
                                    and should be removed before moving into a production environment.</p>
                                <p>Remove test database and access to it? (Press y|Y for Yes, any other key for No)
                                    : <strong>n</strong></p>
                                <p>&mldr; skipping.</p>
                                <p>Reloading the privilege tables will ensure that all changes made so far will take
                                    effect immediately.</p>
                                <p>Reload privilege tables now? (Press y|Y for Yes, any other key for No) :
                                    <strong>y</strong>
                                </p>
                                <p>Success.</p>
                                <p>All done!</p>
                            </blockquote>
                            <p><strong>Note:</strong> The above is Ok for local development, for a production the
                                server the root user should be secured with a
                                strong password and no external access allowed.</p>
                            <h2 id=verify-mysql-is-running class="relative group">Verify MySQL is running <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#verify-mysql-is-running
                                        aria-label=Anchor>#</a></span></h2>
                            <p>To open the MySQL prompt, enter:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>sudo mysql
    </span></span></code></pre>
                            </div>
                            <p>To view the available databases, in the MySQL prompt, enter:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sql data-lang=sql><span class=line><span class=cl><span class=k>SHOW</span><span class=w>
    </span></span></span><span class=line><span class=cl><span class=w></span><span class=n>DATABASES</span><span class=p>;</span><span class=w>
    </span></span></span></code></pre>
                            </div>
                            <p>You should see output like:</p>
                            <blockquote>
                                <table>
                                    <thead>
                                        <tr>
                                            <th style=text-align:left>Database</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr>
                                            <td style=text-align:left>information_schema</td>
                                        </tr>
                                        <tr>
                                            <td style=text-align:left>mysql</td>
                                        </tr>
                                        <tr>
                                            <td style=text-align:left>performance_schema</td>
                                        </tr>
                                        <tr>
                                            <td style=text-align:left>sys</td>
                                        </tr>
                                    </tbody>
                                </table>
                                <p>4 rows in set (0.00 sec)</p>
                            </blockquote>
                            <h2 id=allow-remote-access class="relative group">Allow remote access <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#allow-remote-access
                                        aria-label=Anchor>#</a></span></h2>
                            <p>Allow the root user to assess the database using basic username and password
                                authentication (no certificate required).</p>
                            <p>-
                                Source <a
                                    href=https://stackoverflow.com/questions/58905555/cannot-connect-to-mysql-database-running-on-wsl2-from-windows-desktop-applicat
                                    target=_blank>SO - Cannot connect to MySQL database (running on WSL2) from
                                    Windows Desktop Application “MySQL Workbench”</a></p>
                            <p>From the MySQL prompt run:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sql data-lang=sql><span class=line><span class=cl><span class=k>ALTER</span><span class=w>
    </span></span></span><span class=line><span class=cl><span class=w></span><span class=k>USER</span><span class=w> </span><span class=s1>&#39;root&#39;</span><span class=o>@</span><span class=s1>&#39;localhost&#39;</span><span class=w> </span><span class=n>IDENTIFIED</span><span class=w> </span><span class=k>WITH</span><span class=w> </span><span class=n>mysql_native_password</span><span class=w> </span><span class=k>BY</span><span class=w> </span><span class=s1>&#39;root&#39;</span><span class=p>;</span><span class=w>
    </span></span></span></code></pre>
                            </div>
                            <p>Exit out of the MySQL prompt</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sql data-lang=sql><span class=line><span class=cl><span class=n>exit</span><span class=w>
    </span></span></span></code></pre>
                            </div>
                            <h2 id=configure-heidisql class="relative group">Configure HeidiSQL <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#configure-heidisql
                                        aria-label=Anchor>#</a></span></h2>
                            <p>HeidiSQL can now be configured to connect to MySQL 8.0 in WSL</p>
                            <ul>
                                <li>Click <strong>New</strong> and give the connection a new name, e.g.
                                    <strong>MySQL-WSL2</strong>
                                </li>
                                <li>Library <strong>libmysql.dll</strong></li>
                                <li>Hostname / IP: <strong>localhost</strong></li>
                                <li>User: <strong>root</strong></li>
                                <li>Password: <strong>root</strong></li>
                            </ul>
                            <p>
                            <figure><img class="my-0 rounded-md"
                                    srcset="/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/heidi-sql-setup-for-wsl2_hu28234872cb1352bbd6a5a3b044de0633_23823_330x0_resize_box_3.png 330w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/heidi-sql-setup-for-wsl2_hu28234872cb1352bbd6a5a3b044de0633_23823_660x0_resize_box_3.png 660w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/heidi-sql-setup-for-wsl2_hu28234872cb1352bbd6a5a3b044de0633_23823_1024x0_resize_box_3.png 1024w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/heidi-sql-setup-for-wsl2_hu28234872cb1352bbd6a5a3b044de0633_23823_1320x0_resize_box_3.png 2x"
                                    src=/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/heidi-sql-setup-for-wsl2_hu28234872cb1352bbd6a5a3b044de0633_23823_660x0_resize_box_3.png
                                    alt="HeidiSQL setup for wsl2">
                                <figcaption>HeidiSQL setup for wsl2</figcaption>
                            </figure>
                            </p>
                            <h2 id=change-the-default-port class="relative group">Change the default port <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#change-the-default-port
                                        aria-label=Anchor>#</a></span></h2>
                            <p>MySQL running in Windows (installed by Laragon) and MySQL WSL Ubuntu can not run at
                                the same time using the same port (
                                default 3306), one will need to be changed to avoid conflicts.</p>
                            <p>-
                                source <a
                                    href=https://askubuntu.com/questions/1120540/how-to-change-mysql8-0-default-port
                                    target=_blank>AskUbuntu - How to change mysql8.0 default port</a></p>
                            <p>Open your WSL terminal (ie. Ubuntu).</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-shell data-lang=shell><span class=line><span class=cl>sudo nano /etc/mysql/my.cnf
    </span></span></code></pre>
                            </div>
                            <p>Add these lines to use post 33061:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-ini data-lang=ini><span class=line><span class=cl><span class=k>[mysqld]</span>
    </span></span><span class=line><span class=cl><span class=na>port</span> <span class=o>=</span> <span class=s>33061</span>
    </span></span></code></pre>
                            </div>
                            <p>Save (write) the file and exit nano (CTRL+W enter, CTRL+X)</p>
                            <p>Restart MySQL to load the new config:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-shell data-lang=shell><span class=line><span class=cl>sudo service mysql restart
    </span></span></code></pre>
                            </div>
                            <p>Remember to change the port in HeidiSQL and any .env files.</p>
                            <h2 id=start-mysql-automatically-when-wsl-starts class="relative group">Start MySQL
                                automatically when WSL starts <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important
                                        href=#start-mysql-automatically-when-wsl-starts aria-label=Anchor>#</a></span>
                            </h2>
                            <p>Each time WSL is shutdown MySQL will also stop, when WSL is started MySQL does not
                                automatically start, although it
                                could be configured to do so!</p>
                            <ul>
                                <li>source <a href=https://askubuntu.com/questions/416309/start-mysql-on-startup
                                        target=_blank>AskUbuntu - Start MySQL on Startup</a></li>
                            </ul>
                            <p>Open your WSL terminal (ie. Ubuntu).</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-shell data-lang=shell><span class=line><span class=cl>sudo update-rc.d mysql defaults
    </span></span></code></pre>
                            </div>
                            <p>Now every time WSL is started, MySQL will also start.</p>
                            <h2 id=manually-start-mysql class="relative group">Manually start MySQL <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#manually-start-mysql
                                        aria-label=Anchor>#</a></span></h2>
                            <p>If MySQL is not needed everytime WSL is started, it can be manually started</p>
                            <p>Open your WSL terminal (ie. Ubuntu).</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-shell data-lang=shell><span class=line><span class=cl>sudo service mysql start
    </span></span></code></pre>
                            </div>
                            <h2 id=troubleshooting class="relative group">Troubleshooting <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#troubleshooting
                                        aria-label=Anchor>#</a></span></h2>
                            <h3 id=typical-errors-and-possible-reasons class="relative group">Typical errors and
                                possible reasons <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important
                                        href=#typical-errors-and-possible-reasons aria-label=Anchor>#</a></span>
                            </h3>
                            <p>This is the error when MySQL is not running:</p>
                            <p>
                            <figure><img class="my-0 rounded-md"
                                    srcset="/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/cant-connect_hu877783165be040d098b29d9b96434ce9_3600_330x0_resize_box_3.png 330w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/cant-connect_hu877783165be040d098b29d9b96434ce9_3600_660x0_resize_box_3.png 660w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/cant-connect_hu877783165be040d098b29d9b96434ce9_3600_1024x0_resize_box_3.png 1024w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/cant-connect_hu877783165be040d098b29d9b96434ce9_3600_1320x0_resize_box_3.png 2x"
                                    src=/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/cant-connect_hu877783165be040d098b29d9b96434ce9_3600_660x0_resize_box_3.png
                                    alt="Can&amp;rsquo;t connect to MySQL server">
                                <figcaption>Can&rsquo;t connect to MySQL server</figcaption>
                            </figure>
                            </p>
                            <p>When you try to connect without using a password (note: using password:
                                <strong>NO</strong>)
                            </p>
                            <p>
                            <figure><img class="my-0 rounded-md"
                                    srcset="/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/access-denied-using-password-no_hu822ee80bb9ccc7c20922f2bcfe629024_3970_330x0_resize_box_3.png 330w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/access-denied-using-password-no_hu822ee80bb9ccc7c20922f2bcfe629024_3970_660x0_resize_box_3.png 660w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/access-denied-using-password-no_hu822ee80bb9ccc7c20922f2bcfe629024_3970_1024x0_resize_box_3.png 1024w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/access-denied-using-password-no_hu822ee80bb9ccc7c20922f2bcfe629024_3970_1320x0_resize_box_3.png 2x"
                                    src=/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/access-denied-using-password-no_hu822ee80bb9ccc7c20922f2bcfe629024_3970_660x0_resize_box_3.png
                                    alt="Access denied using password NO">
                                <figcaption>Access denied using password NO</figcaption>
                            </figure>
                            </p>
                            <p>When you try to connect using the wrong password (note: using password:
                                <strong>YES</strong>):
                            </p>
                            <p>
                            <figure><img class="my-0 rounded-md"
                                    srcset="/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/access-denied-using-password-yes_hu653ffce3966709396a9af5e3abf8c6ef_3982_330x0_resize_box_3.png 330w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/access-denied-using-password-yes_hu653ffce3966709396a9af5e3abf8c6ef_3982_660x0_resize_box_3.png 660w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/access-denied-using-password-yes_hu653ffce3966709396a9af5e3abf8c6ef_3982_1024x0_resize_box_3.png 1024w,
    /2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/access-denied-using-password-yes_hu653ffce3966709396a9af5e3abf8c6ef_3982_1320x0_resize_box_3.png 2x"
                                    src=/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/images/access-denied-using-password-yes_hu653ffce3966709396a9af5e3abf8c6ef_3982_660x0_resize_box_3.png
                                    alt="Access denied using password YES">
                                <figcaption>Access denied using password YES</figcaption>
                            </figure>
                            </p>
                            <h3 id=basic-troubleshooting class="relative group">Basic troubleshooting <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#basic-troubleshooting
                                        aria-label=Anchor>#</a></span></h3>
                            <p>Basic troubleshooting steps can be performed</p>
                            <ul>
                                <li>
                                    <p>make sure WSL is started</p>
                                </li>
                                <li>
                                    <p>check the MySQL server is running</p>
                                </li>
                                <li>
                                    <p>source and more
                                        information <a
                                            href=https://upcloud.com/community/tutorials/fix-common-problems-mysql-databases/
                                            target=_blank>How to fix common problems with MySQL databases</a></p>
                                </li>
                            </ul>
                            <h3 id=check-the-server-is-running class="relative group">Check the server is running
                                <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#check-the-server-is-running
                                        aria-label=Anchor>#</a></span>
                            </h3>
                            <p>Check that the service is running If your website cannot connect to your database, it
                                is possible the service is simply
                                not listening. Check your MySQL state, on Ubuntu and Debian systems this can be done
                                with the following command.</p>
                            <p>Open your WSL terminal (ie. Ubuntu).</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>sudo service mysql status
    </span></span></code></pre>
                            </div>
                            <p>When running the stats will be displayed:</p>
                            <blockquote>
                                <ul>
                                    <li>/usr/bin/mysqladmin Ver 8.0.26-0ubuntu0.20.04.2 for Linux on x86_64
                                        ((Ubuntu))
                                        > Copyright (c) 2000, 2021, Oracle and/or its affiliates.</li>
                                </ul>
                                <p>Oracle is a registered trademark of Oracle Corporation and/or its
                                    affiliates. Other names may be trademarks of their respective
                                    owners.</p>
                                <p>Server version 8.0.26-0ubuntu0.20.04.2
                                    Protocol version 10
                                    Connection Localhost via UNIX socket
                                    UNIX socket /var/run/mysqld/mysqld.sock
                                    Uptime: 2 hours 12 min 21 sec</p>
                                <p>Threads: 2 Questions: 743 Slow queries: 0 Opens: 598 Flush tables: 3 Open tables:
                                    151<br>Queries per second avg: 0.093</p>
                            </blockquote>
                            <p>If MySQL is not running it will simply show:</p>
                            <blockquote>
                                <ul>
                                    <li>MySQL is stopped.</li>
                                </ul>
                            </blockquote>
                            <p>If your service status says something other than ‘running’, try to restart the
                                process using the same service command as
                                before but with ‘restart’ instead of ‘status’.</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>sudo service mysql restart
    </span></span></code></pre>
                            </div>
                            <blockquote>
                                <ul>
                                    <li>Stopping MySQL database server mysqld [ OK ]</li>
                                    <li>Starting MySQL database server mysqld<br>> su: warning: cannot change
                                        directory to /nonexistent: No such file or
                                        directory [ OK ]</li>
                                </ul>
                            </blockquote>
                            <p>To fix the error with <strong>/nonexistent</strong> directory:</p>
                            <p>-
                                Source <a
                                    href=https://serverfault.com/questions/755763/mysql-nonexistent-home-vs-no-directory-logging-in-with-home/942422
                                    target=_blank>serverfault.com - MySQL /nonexistent home vs “No directory,
                                    logging in with HOME=/”</a></p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>sudo service mysql stop
    </span></span><span class=line><span class=cl>sudo usermod -d /var/lib/mysql/ mysql
    </span></span><span class=line><span class=cl>sudo service mysql start
    </span></span></code></pre>
                            </div>
                            <h3 id=check-the-mysql-server-for-errors class="relative group">Check the MySQL server
                                for errors <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important
                                        href=#check-the-mysql-server-for-errors aria-label=Anchor>#</a></span></h3>
                            <p>To check the error log:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>sudo grep -i error /var/log/mysql/error.log
    </span></span></code></pre>
                            </div>
                            <p>Example output:</p>
                            <blockquote>
                                <p>2021-08-08T16:45:10.239063Z 0 [ERROR] [MY-010257] [Server] Do you already have
                                    another mysqld server running on port:
                                    33060 ?</p>
                            </blockquote>
                            <p>This error happened when I tried to set port=33060, the MySQL server failed to
                                restart, which I way I used 33061 😉</p>
                            <h3 id=check-the-port class="relative group">Check the port <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#check-the-port
                                        aria-label=Anchor>#</a></span></h3>
                            <p>-
                                source: <a
                                    href=https://serverfault.com/questions/116100/how-to-check-what-port-mysql-is-running-on
                                    target=_blank>ServerFault - How to check what port mysql is running on</a></p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-shell data-lang=shell><span class=line><span class=cl>mysql -u root -p
    </span></span></code></pre>
                            </div>
                            <p>Enter the root password set earlier (e.g. <strong>root</strong>)</p>
                            <p>From the <code>mysql></code> prompt:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sql data-lang=sql><span class=line><span class=cl><span class=k>SHOW</span><span class=w>
    </span></span></span><span class=line><span class=cl><span class=w></span><span class=k>GLOBAL</span><span class=w> </span><span class=n>VARIABLES</span><span class=w> </span><span class=k>LIKE</span><span class=w> </span><span class=s1>&#39;PORT&#39;</span><span class=p>;</span><span class=w>
    </span></span></span></code></pre>
                            </div>
                            <p>The output will show the configured port, e.g.:</p>
                            <blockquote>
                                <table>
                                    <thead>
                                        <tr>
                                            <th style=text-align:left>Database</th>
                                            <th style=text-align:left>Value</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr>
                                            <td style=text-align:left>port</td>
                                            <td style=text-align:left>33061</td>
                                        </tr>
                                    </tbody>
                                </table>
                                <p>1 row in set (0.00 sec)</p>
                            </blockquote>
                            <h2 id=stop-mysql-and-wsl class="relative group">Stop MySQL and WSL <span
                                    class="absolute top-0 w-6 transition-opacity opacity-0 ltr:-left-6 rtl:-right-6 not-prose group-hover:opacity-100"><a
                                        class="group-hover:text-primary-300 dark:group-hover:text-neutral-700"
                                        style=text-decoration-line:none!important href=#stop-mysql-and-wsl
                                        aria-label=Anchor>#</a></span></h2>
                            <p>To close WSL</p>
                            <p>If you are in the WSL terminal (i.e. Ubuntu).</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-shell data-lang=shell><span class=line><span class=cl><span class=nb>exit</span>
    </span></span></code></pre>
                            </div>
                            <p>Open PowerShell and run:</p>
                            <div class=highlight>
                                <pre tabindex=0 class=chroma><code class=language-sh data-lang=sh><span class=line><span class=cl>wsl --shutdown
    </span></span></code></pre>
                            </div>
                        </div>
                    </section>
                    <footer class="pt-8 max-w-prose print:hidden">
                        <div class=flex><img class="!mt-0 !mb-0 h-24 w-24 rounded-full ltr:mr-4 rtl:ml-4" width=96
                                height=96 alt="Michael Pritchard (AKA Pen-y-Fan)"
                                src=/img/author_hu5893a0bcecd6e8dce34dc674da9c7f56_6794556_192x192_fill_q75_box_smart1.jpg>
                            <div class=place-self-center>
                                <div class="text-[0.6rem] uppercase leading-3 text-neutral-500 dark:text-neutral-400">
                                    Author</div>
                                <div class="font-semibold leading-6 text-neutral-800 dark:text-neutral-300">Michael
                                    Pritchard (AKA Pen-y-Fan)</div>
                                <div class="text-sm text-neutral-700 dark:text-neutral-400">Posts about my coding
                                    experiences, primarily for my learning path with PHP, good clean professional
                                    code.</div>
                                <div class="text-2xl sm:text-lg">
                                    <div class="flex flex-wrap text-neutral-400 dark:text-neutral-500"><a
                                            class="px-1 transition-transform hover:scale-125 hover:text-primary-700 dark:hover:text-primary-400"
                                            href=https://github.com/Pen-y-Fan target=_blank aria-label=Github
                                            rel="me noopener noreferrer"><span
                                                class="relative inline-block align-text-bottom icon"><svg
                                                    xmlns="http://www.w3.org/2000/svg" viewBox="0 0 496 512">
                                                    <path fill="currentcolor"
                                                        d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3.3-5.6-1.3-5.6-3.6.0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6.0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5.3-6.2 2.3zm44.2-1.7c-2.9.7-4.9 2.6-4.6 4.9.3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3.0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1.0-6.2-.3-40.4-.3-61.4.0.0-70 15-84.7-29.8.0.0-11.4-29.1-27.8-36.6.0.0-22.9-15.7 1.6-15.4.0.0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5.0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9.0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4.0 33.7-.3 75.4-.3 83.6.0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3.7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3.3 2.9 2.3 3.9 1.6 1 3.6.7 4.3-.7.7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3.7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3.7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6.0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9.0-6.2-1.4-2.3-4-3.3-5.6-2z" />
                                                </svg></span></a><a
                                            class="px-1 transition-transform hover:scale-125 hover:text-primary-700 dark:hover:text-primary-400"
                                            href=https://www.linkedin.com/in/michael-pritchard-340b4520/ target=_blank
                                            aria-label=Linkedin rel="me noopener noreferrer"><span
                                                class="relative inline-block align-text-bottom icon"><svg
                                                    xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">
                                                    <path fill="currentcolor"
                                                        d="M416 32H31.9C14.3 32 0 46.5.0 64.3v383.4C0 465.5 14.3 480 31.9 480H416c17.6.0 32-14.5 32-32.3V64.3c0-17.8-14.4-32.3-32-32.3zM135.4 416H69V202.2h66.5V416zm-33.2-243c-21.3.0-38.5-17.3-38.5-38.5S80.9 96 102.2 96c21.2.0 38.5 17.3 38.5 38.5.0 21.3-17.2 38.5-38.5 38.5zm282.1 243h-66.4V312c0-24.8-.5-56.7-34.5-56.7-34.6.0-39.9 27-39.9 54.9V416h-66.4V202.2h63.7v29.2h.9c8.9-16.8 30.6-34.5 62.9-34.5 67.2.0 79.7 44.3 79.7 101.9V416z" />
                                                </svg></span></a><a
                                            class="px-1 transition-transform hover:scale-125 hover:text-primary-700 dark:hover:text-primary-400"
                                            href=https://www.reddit.com/user/Pen-y-Fan target=_blank aria-label=Reddit
                                            rel="me noopener noreferrer"><span
                                                class="relative inline-block align-text-bottom icon"><svg
                                                    xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                                                    <path fill="currentcolor"
                                                        d="M201.5 305.5c-13.8.0-24.9-11.1-24.9-24.6.0-13.8 11.1-24.9 24.9-24.9 13.6.0 24.6 11.1 24.6 24.9.0 13.6-11.1 24.6-24.6 24.6zM504 256c0 137-111 248-248 248S8 393 8 256 119 8 256 8s248 111 248 248zm-132.3-41.2c-9.4.0-17.7 3.9-23.8 10-22.4-15.5-52.6-25.5-86.1-26.6l17.4-78.3 55.4 12.5c0 13.6 11.1 24.6 24.6 24.6 13.8.0 24.9-11.3 24.9-24.9s-11.1-24.9-24.9-24.9c-9.7.0-18 5.8-22.1 13.8l-61.2-13.6c-3-.8-6.1 1.4-6.9 4.4l-19.1 86.4c-33.2 1.4-63.1 11.3-85.5 26.8-6.1-6.4-14.7-10.2-24.1-10.2-34.9.0-46.3 46.9-14.4 62.8-1.1 5-1.7 10.2-1.7 15.5.0 52.6 59.2 95.2 132 95.2 73.1.0 132.3-42.6 132.3-95.2.0-5.3-.6-10.8-1.9-15.8 31.3-16 19.8-62.5-14.9-62.5zM302.8 331c-18.2 18.2-76.1 17.9-93.6.0-2.2-2.2-6.1-2.2-8.3.0-2.5 2.5-2.5 6.4.0 8.6 22.8 22.8 87.3 22.8 110.2.0 2.5-2.2 2.5-6.1.0-8.6-2.2-2.2-6.1-2.2-8.3.0zm7.7-75c-13.6.0-24.6 11.1-24.6 24.9.0 13.6 11.1 24.6 24.6 24.6 13.8.0 24.9-11.1 24.9-24.6.0-13.8-11-24.9-24.9-24.9z" />
                                                </svg></span></a><a
                                            class="px-1 transition-transform hover:scale-125 hover:text-primary-700 dark:hover:text-primary-400"
                                            href=https://twitter.com/MikeAPritch target=_blank aria-label=Twitter
                                            rel="me noopener noreferrer"><span
                                                class="relative inline-block align-text-bottom icon"><svg
                                                    xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                                                    <path fill="currentcolor"
                                                        d="M459.37 151.716c.325 4.548.325 9.097.325 13.645.0 138.72-105.583 298.558-298.558 298.558-59.452.0-114.68-17.219-161.137-47.106 8.447.974 16.568 1.299 25.34 1.299 49.055.0 94.213-16.568 130.274-44.832-46.132-.975-84.792-31.188-98.112-72.772 6.498.974 12.995 1.624 19.818 1.624 9.421.0 18.843-1.3 27.614-3.573-48.081-9.747-84.143-51.98-84.143-102.985v-1.299c13.969 7.797 30.214 12.67 47.431 13.319-28.264-18.843-46.781-51.005-46.781-87.391.0-19.492 5.197-37.36 14.294-52.954 51.655 63.675 129.3 105.258 216.365 109.807-1.624-7.797-2.599-15.918-2.599-24.04.0-57.828 46.782-104.934 104.934-104.934 30.213.0 57.502 12.67 76.67 33.137 23.715-4.548 46.456-13.32 66.599-25.34-7.798 24.366-24.366 44.833-46.132 57.827 21.117-2.273 41.584-8.122 60.426-16.243-14.292 20.791-32.161 39.308-52.628 54.253z" />
                                                </svg></span></a></div>
                                </div>
                            </div>
                        </div>
                        <section class="flex flex-row flex-wrap justify-center pt-4 text-xl"><a
                                class="m-1 inline-block min-w-[2.4rem] rounded bg-neutral-300 p-1 text-center text-neutral-700 hover:bg-primary-500 hover:text-neutral dark:bg-neutral-700 dark:text-neutral-300 dark:hover:bg-primary-400 dark:hover:text-neutral-800"
                                href="https://www.facebook.com/sharer/sharer.php?u=https://pen-y-fan.github.io/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/&amp;quote=How%20to%20install%20MySQL%20on%20WSL%202%20%28Ubuntu%29"
                                title="Share on Facebook" aria-label="Share on Facebook"><span
                                    class="relative inline-block align-text-bottom icon"><svg
                                        xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                                        <path fill="currentcolor"
                                            d="M504 256C504 119 393 8 256 8S8 119 8 256c0 123.78 90.69 226.38 209.25 245V327.69h-63V256h63v-54.64c0-62.15 37-96.48 93.67-96.48 27.14.0 55.52 4.84 55.52 4.84v61h-31.28c-30.8.0-40.41 19.12-40.41 38.73V256h68.78l-11 71.69h-57.78V501C413.31 482.38 504 379.78 504 256z" />
                                    </svg></span></a><a
                                class="m-1 inline-block min-w-[2.4rem] rounded bg-neutral-300 p-1 text-center text-neutral-700 hover:bg-primary-500 hover:text-neutral dark:bg-neutral-700 dark:text-neutral-300 dark:hover:bg-primary-400 dark:hover:text-neutral-800"
                                href="https://twitter.com/intent/tweet/?url=https://pen-y-fan.github.io/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/&amp;text=How%20to%20install%20MySQL%20on%20WSL%202%20%28Ubuntu%29"
                                title="Tweet on Twitter" aria-label="Tweet on Twitter"><span
                                    class="relative inline-block align-text-bottom icon"><svg
                                        xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                                        <path fill="currentcolor"
                                            d="M459.37 151.716c.325 4.548.325 9.097.325 13.645.0 138.72-105.583 298.558-298.558 298.558-59.452.0-114.68-17.219-161.137-47.106 8.447.974 16.568 1.299 25.34 1.299 49.055.0 94.213-16.568 130.274-44.832-46.132-.975-84.792-31.188-98.112-72.772 6.498.974 12.995 1.624 19.818 1.624 9.421.0 18.843-1.3 27.614-3.573-48.081-9.747-84.143-51.98-84.143-102.985v-1.299c13.969 7.797 30.214 12.67 47.431 13.319-28.264-18.843-46.781-51.005-46.781-87.391.0-19.492 5.197-37.36 14.294-52.954 51.655 63.675 129.3 105.258 216.365 109.807-1.624-7.797-2.599-15.918-2.599-24.04.0-57.828 46.782-104.934 104.934-104.934 30.213.0 57.502 12.67 76.67 33.137 23.715-4.548 46.456-13.32 66.599-25.34-7.798 24.366-24.366 44.833-46.132 57.827 21.117-2.273 41.584-8.122 60.426-16.243-14.292 20.791-32.161 39.308-52.628 54.253z" />
                                    </svg></span></a><a
                                class="m-1 inline-block min-w-[2.4rem] rounded bg-neutral-300 p-1 text-center text-neutral-700 hover:bg-primary-500 hover:text-neutral dark:bg-neutral-700 dark:text-neutral-300 dark:hover:bg-primary-400 dark:hover:text-neutral-800"
                                href="https://pinterest.com/pin/create/bookmarklet/?url=https://pen-y-fan.github.io/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/&amp;description=How%20to%20install%20MySQL%20on%20WSL%202%20%28Ubuntu%29"
                                title="Pin on Pinterest" aria-label="Pin on Pinterest"><span
                                    class="relative inline-block align-text-bottom icon"><svg
                                        xmlns="http://www.w3.org/2000/svg" viewBox="0 0 496 512">
                                        <path fill="currentcolor"
                                            d="M496 256c0 137-111 248-248 248-25.6.0-50.2-3.9-73.4-11.1 10.1-16.5 25.2-43.5 30.8-65 3-11.6 15.4-59 15.4-59 8.1 15.4 31.7 28.5 56.8 28.5 74.8.0 128.7-68.8 128.7-154.3.0-81.9-66.9-143.2-152.9-143.2-107 0-163.9 71.8-163.9 150.1.0 36.4 19.4 81.7 50.3 96.1 4.7 2.2 7.2 1.2 8.3-3.3.8-3.4 5-20.3 6.9-28.1.6-2.5.3-4.7-1.7-7.1-10.1-12.5-18.3-35.3-18.3-56.6.0-54.7 41.4-107.6 112-107.6 60.9.0 103.6 41.5 103.6 100.9.0 67.1-33.9 113.6-78 113.6-24.3.0-42.6-20.1-36.7-44.8 7-29.5 20.5-61.3 20.5-82.6.0-19-10.2-34.9-31.4-34.9-24.9.0-44.9 25.7-44.9 60.2.0 22 7.4 36.8 7.4 36.8s-24.5 103.8-29 123.2c-5 21.4-3 51.6-.9 71.2C65.4 450.9.0 361.1.0 256 0 119 111 8 248 8s248 111 248 248z" />
                                    </svg></span></a><a
                                class="m-1 inline-block min-w-[2.4rem] rounded bg-neutral-300 p-1 text-center text-neutral-700 hover:bg-primary-500 hover:text-neutral dark:bg-neutral-700 dark:text-neutral-300 dark:hover:bg-primary-400 dark:hover:text-neutral-800"
                                href="https://reddit.com/submit/?url=https://pen-y-fan.github.io/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/&amp;resubmit=true&amp;title=How%20to%20install%20MySQL%20on%20WSL%202%20%28Ubuntu%29"
                                title="Submit to Reddit" aria-label="Submit to Reddit"><span
                                    class="relative inline-block align-text-bottom icon"><svg
                                        xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                                        <path fill="currentcolor"
                                            d="M201.5 305.5c-13.8.0-24.9-11.1-24.9-24.6.0-13.8 11.1-24.9 24.9-24.9 13.6.0 24.6 11.1 24.6 24.9.0 13.6-11.1 24.6-24.6 24.6zM504 256c0 137-111 248-248 248S8 393 8 256 119 8 256 8s248 111 248 248zm-132.3-41.2c-9.4.0-17.7 3.9-23.8 10-22.4-15.5-52.6-25.5-86.1-26.6l17.4-78.3 55.4 12.5c0 13.6 11.1 24.6 24.6 24.6 13.8.0 24.9-11.3 24.9-24.9s-11.1-24.9-24.9-24.9c-9.7.0-18 5.8-22.1 13.8l-61.2-13.6c-3-.8-6.1 1.4-6.9 4.4l-19.1 86.4c-33.2 1.4-63.1 11.3-85.5 26.8-6.1-6.4-14.7-10.2-24.1-10.2-34.9.0-46.3 46.9-14.4 62.8-1.1 5-1.7 10.2-1.7 15.5.0 52.6 59.2 95.2 132 95.2 73.1.0 132.3-42.6 132.3-95.2.0-5.3-.6-10.8-1.9-15.8 31.3-16 19.8-62.5-14.9-62.5zM302.8 331c-18.2 18.2-76.1 17.9-93.6.0-2.2-2.2-6.1-2.2-8.3.0-2.5 2.5-2.5 6.4.0 8.6 22.8 22.8 87.3 22.8 110.2.0 2.5-2.2 2.5-6.1.0-8.6-2.2-2.2-6.1-2.2-8.3.0zm7.7-75c-13.6.0-24.6 11.1-24.6 24.9.0 13.6 11.1 24.6 24.6 24.6 13.8.0 24.9-11.1 24.9-24.6.0-13.8-11-24.9-24.9-24.9z" />
                                    </svg></span></a><a
                                class="m-1 inline-block min-w-[2.4rem] rounded bg-neutral-300 p-1 text-center text-neutral-700 hover:bg-primary-500 hover:text-neutral dark:bg-neutral-700 dark:text-neutral-300 dark:hover:bg-primary-400 dark:hover:text-neutral-800"
                                href="https://www.linkedin.com/shareArticle?mini=true&amp;url=https://pen-y-fan.github.io/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/&amp;title=How%20to%20install%20MySQL%20on%20WSL%202%20%28Ubuntu%29"
                                title="Share on LinkedIn" aria-label="Share on LinkedIn"><span
                                    class="relative inline-block align-text-bottom icon"><svg
                                        xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">
                                        <path fill="currentcolor"
                                            d="M416 32H31.9C14.3 32 0 46.5.0 64.3v383.4C0 465.5 14.3 480 31.9 480H416c17.6.0 32-14.5 32-32.3V64.3c0-17.8-14.4-32.3-32-32.3zM135.4 416H69V202.2h66.5V416zm-33.2-243c-21.3.0-38.5-17.3-38.5-38.5S80.9 96 102.2 96c21.2.0 38.5 17.3 38.5 38.5.0 21.3-17.2 38.5-38.5 38.5zm282.1 243h-66.4V312c0-24.8-.5-56.7-34.5-56.7-34.6.0-39.9 27-39.9 54.9V416h-66.4V202.2h63.7v29.2h.9c8.9-16.8 30.6-34.5 62.9-34.5 67.2.0 79.7 44.3 79.7 101.9V416z" />
                                    </svg></span></a><a
                                class="m-1 inline-block min-w-[2.4rem] rounded bg-neutral-300 p-1 text-center text-neutral-700 hover:bg-primary-500 hover:text-neutral dark:bg-neutral-700 dark:text-neutral-300 dark:hover:bg-primary-400 dark:hover:text-neutral-800"
                                href="mailto:?body=https://pen-y-fan.github.io/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/&amp;subject=How%20to%20install%20MySQL%20on%20WSL%202%20%28Ubuntu%29"
                                title="Send via email" aria-label="Send via email"><span
                                    class="relative inline-block align-text-bottom icon"><svg
                                        xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                                        <path fill="currentcolor"
                                            d="M207.8 20.73c-93.45 18.32-168.7 93.66-187 187.1-27.64 140.9 68.65 266.2 199.1 285.1 19.01 2.888 36.17-12.26 36.17-31.49l1e-4-.6631c0-15.74-11.44-28.88-26.84-31.24-84.35-12.98-149.2-86.13-149.2-174.2.0-102.9 88.61-185.5 193.4-175.4 91.54 8.869 158.6 91.25 158.6 183.2v16.16c0 22.09-17.94 40.05-40 40.05s-40.01-17.96-40.01-40.05v-120.1c0-8.847-7.161-16.02-16.01-16.02l-31.98.0036c-7.299.0-13.2 4.992-15.12 11.68-24.85-12.15-54.24-16.38-86.06-5.106-38.75 13.73-68.12 48.91-73.72 89.64-9.483 69.01 43.81 128 110.9 128 26.44.0 50.43-9.544 69.59-24.88 24 31.3 65.23 48.69 109.4 37.49C465.2 369.3 496 324.1 495.1 277.2V256.3c0-149.2-133.9-265.632-287.3-235.57zM239.1 304.3c-26.47.0-48-21.56-48-48.05s21.53-48.05 48-48.05 48 21.56 48 48.05-20.6 48.05-48 48.05z" />
                                    </svg></span></a></section>
                        <div class=pt-8>
                            <hr class="border-dotted border-neutral-300 dark:border-neutral-600">
                            <div class="flex justify-between pt-3"><span><a class="group flex"
                                        href=/2021/08/03/How-to-Set-up-VS-Code-to-use-PHP-with-Xdebug-3-on-Windows /><span
                                        class="mr-2 text-neutral-700 transition-transform group-hover:-translate-x-[2px] group-hover:text-primary-600 ltr:inline rtl:hidden dark:text-neutral dark:group-hover:text-primary-400">&larr;</span>
                                    <span
                                        class="ml-2 text-neutral-700 transition-transform group-hover:translate-x-[2px] group-hover:text-primary-600 ltr:hidden rtl:inline dark:text-neutral dark:group-hover:text-primary-400">&rarr;</span>
                                    <span class="flex flex-col"><span
                                            class="mt-[0.1rem] leading-6 group-hover:underline group-hover:decoration-primary-500">How
                                            to Set up VS Code to use PHP with Xdebug 3 on Windows</span>
                                        <span class="mt-[0.1rem] text-xs text-neutral-500 dark:text-neutral-400"><time
                                                datetime="2021-08-03 22:57:39 +0000 UTC">3 August
                                                2021</time></span></span></a></span>
                                <span><a class="group flex text-right"
                                        href=/2021/09/14/How-to-send-Emails-using-PHPMailer-from-Windows /><span
                                        class="flex flex-col"><span
                                            class="mt-[0.1rem] leading-6 group-hover:underline group-hover:decoration-primary-500">How
                                            to send Emails using PHPMailer from Windows</span>
                                        <span class="mt-[0.1rem] text-xs text-neutral-500 dark:text-neutral-400"><time
                                                datetime="2021-09-14 19:44:04 +0000 UTC">14 September
                                                2021</time></span></span>
                                    <span
                                        class="ml-2 text-neutral-700 transition-transform group-hover:translate-x-[2px] group-hover:text-primary-600 ltr:inline rtl:hidden dark:text-neutral dark:group-hover:text-primary-400">&rarr;</span>
                                    <span
                                        class="mr-2 text-neutral-700 transition-transform group-hover:-translate-x-[2px] group-hover:text-primary-600 ltr:hidden rtl:inline dark:text-neutral dark:group-hover:text-primary-400">&larr;</span></a></span>
                            </div>
                        </div>
                    </footer>
                </article>
                <div class="pointer-events-none absolute top-[100vh] bottom-0 w-12 ltr:right-0 rtl:left-0"><a
                        href=#the-top
                        class="pointer-events-auto sticky top-[calc(100vh-5.5rem)] flex h-12 w-12 items-center justify-center rounded-full bg-neutral/50 text-xl text-neutral-700 backdrop-blur hover:text-primary-600 dark:bg-neutral-800/50 dark:text-neutral dark:hover:text-primary-400"
                        aria-label="Scroll to top" title="Scroll to top">&uarr;</a></div>
            </main>
        </div>
