## Rhodesとは

iOSとAndroidのクロスプラットフォーム開発が行える、rubyのSDK

## インストール

※rubyがinstall済み(作業時1.9.0使用)

```console
# gem install Rhodes
```

#### android SDKやNDK等のパスを聞いてくるのでパスを入力してやる

## rhodes-setup

```console
We will ask you a few questions below about your dev environment.

JDK path (required) (/Library/Java/Home):
Android SDK path (blank to skip) (): /Users/hoge/android-sdk-macosx/
Android NDK path (blank to skip) ():
Windows Mobile 6 SDK CabWiz (blank to skip) ():
BlackBerry JDE 4.6 (blank to skip) ():
BlackBerry JDE 4.6 MDS (blank to skip) ():
BlackBerry JDE 4.2 (blank to skip) ():
BlackBerry JDE 4.2 MDS (blank to skip) ():

If you want to build with other BlackBerry SDK versions edit: /Users/hoge/.rvm/gems/ruby-1.9.3-p0/gems/rhodes-3.5.1.12/rhobuild.yml
```

#### railsチックに構成される

```console
#rhodes app myapp
/Users/nishikioritatsunori/.rvm/rubies/ruby-1.9.3-p0/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:55:in `require': iconv will be deprecated in the future, use String#encode instead.
Generating with app generator:
     [ADDED]  myapp/rhoconfig.txt
     [ADDED]  myapp/build.yml
     [ADDED]  myapp/.gitignore
     [ADDED]  myapp/app/application.rb
     [ADDED]  myapp/app/index.erb
     [ADDED]  myapp/app/index.bb.erb
     [ADDED]  myapp/app/layout.erb
     [ADDED]  myapp/app/loading.html
     [ADDED]  myapp/Rakefile
     [ADDED]  myapp/app/loading.png
     [ADDED]  myapp/app/loading-568h@2x.png
     [ADDED]  myapp/app/loading-Landscape.png
     [ADDED]  myapp/app/loading-LandscapeLeft.png
     [ADDED]  myapp/app/loading-LandscapeRight.png
     [ADDED]  myapp/app/loading-Portrait.png
     [ADDED]  myapp/app/loading-PortraitUpsideDown.png
     [ADDED]  myapp/app/loading@2x.png
     [ADDED]  myapp/app/helpers
     [ADDED]  myapp/icon
     [ADDED]  myapp/app/Settings
     [ADDED]  myapp/public
```

```console
#rake switch_app
#rake build:iphone:setup_xcode_project
```

### buildされたplatform

/Users/hoge/.rvm/gems/ruby-1.9.2-p290/gems/rhodes-3.0.2/platform/iphone

### エミュレータ実行

```console
#rake run:iphone
```
