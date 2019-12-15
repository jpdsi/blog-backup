# hexo-theme-jpazure

hexo theme for jpazure tech blog

> forked from [hexo-theme-landscape](https://github.com/hexojs/hexo-theme-landscape)

## Set Up

```sh
git clone https://github.com/jpazureid/hexo-theme-jpazure.git themes/jpazure
npm i -D hexo-generator-feed
npm i -D jpazureid/hexo-helper-github-issues hexo-generator-root-tag
```

## hexo config.yml


```yml
root_tag_generator:
  root_tag_names:
    - Azure AD
    - AAD Connect
    - AD FS
    - 情報採取
  sub_tag_limit: 20
  
github:
  url: https://github.com/jpazureid/blog/
  posts_dir: articles

```
