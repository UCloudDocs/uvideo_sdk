{{indexmenu_n>3}}

\#\#\# 安装 Cocoapods

如果您已安装 Cocoapods，则请直接跳过该步骤，直接进入下一步骤。 如果你未接触过 Cocoapods
，需了解Cocoapods并进行安装。也可以直接根据我们提供的简要步骤，进行安装。

简要步骤：打开mac自带的 终端(terminal)，然后输入依次执行下述命令。

\`\`\`bash \# 注释：Ruby-China 推荐2.6.x，实际 mac 自带的 ruby 也能用了 gem sources
--add <https://gems.ruby-china.com/> --remove <https://rubygems.org/>
gem sources -l \# 注释：上面的命令，应该会输出以下内容，\>\>\> 代表此处为输出

> > > <https://gems.ruby-china.com>

\# 注释：确保只有 gems.ruby-china.com

sudo gem install cocoapods \# 注释：由于我们不需要使用官方库，所以可以不执行 pod setup。 \`\`\`

\#\#\# 使用Podfile集成

通过 \[CocoaPods\](<https://cocoapods.org/>) 安装可以最大化地简化安装过程。 首先，在项目根目录下的
Podfile 文件中添加以下 pods（我们假设您的项目 target 名称为 \`iOSDemo\`）：

\`\`\`ruby target 'iOSDemo' do

``` 
  pod 'MovieousShortVideo'
```

end \`\`\`

\<span data-type="color" style="color:rgb(51, 51, 51)"\>\<span
data-type="background" style="background-color:rgb(255, 255,
255)"\>然后在项目根目录执行 \</span\>\</span\>\`pod install\`\<span
data-type="color" style="color:rgb(51, 51, 51)"\>\<span
data-type="background" style="background-color:rgb(255, 255, 255)"\>
\</span\>\</span\>命令，执行成功后，SDK 就集成到项目中了。 如果长时间没有拉取过pod
仓库，可能出现无法找到我们的repo的情况，此时建议先使用如下方式更新pod仓库。`pod
repo update`

\#\#\# SDK 支持情况：

支持 iOS8 及其以上版本。
