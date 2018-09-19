FROM microsoft/windowsservercore:ltsc2016

# Set powershell as default
SHELL ["powershell"]

# Download Cygwin Installer
RUN "mkdir C:\temp ;\
  cd C:\temp ;\
  wget https://cygwin.com/setup-x86_64.exe -UseBasicParsing -OutFile cygwin.exe"

# Install Cygwin and its devel packages
RUN "C:\temp\cygwin.exe \
  --quiet-mode \
  --download \
  --local-install \
  --delete-orphans \
  --site http://mirrors.kernel.org/sourceware/cygwin/ \
  --local-package-dir C:\cygwin_packages \
  --root C:\cygwin64 \
  --categories Devel"

# Install Cygwin build extra packages
RUN "C:\temp\cygwin.exe -q -P \
  autoconf \
  make \
  unzip \
  zip \
  cpio"

# Install Cygwin test extra packages
RUN "C:\temp\cygwin.exe -q -P \
  curl \
  curl-debuginfo \
  libcurl-devel \
  libpng15 \
  libpng-devel \
  perl-Text-CSV \
  xinit \
  xorg-server \
  xterm"

# Download and extract grep-2.27-2
RUN "cd C:\cygwin64\tmp ;\
  wget http://mirrors.kernel.org/sourceware/cygwin/x86_64/release/grep/grep-2.27-2.tar.xz -UseBasicParsing -OutFile grep.tar.xz ;\
  C:\cygwin64\bin\bash.exe -l -c 'tar -C / -xvf /tmp/grep.tar.xz'"

# Add C:\cygwin\bin to the start of the path
RUN "$oldpath=(Get-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment' -Name PATH).path ;\
  $newpath='C:\cygwin64\bin;$oldpath' ;\
  Set-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment' -Name PATH -Value $newpath"
