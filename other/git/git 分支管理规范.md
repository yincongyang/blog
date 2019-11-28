### 分支用途和规范
> 长期分支为master & dev,其余均为临时分支，定期删除

- master: 发布分支，只能从release分支，hotfix分支合并过来，每次PR都必须对应一个release版本
```
提交权限：无
合并权限：管理员 & PM & 项目负责人(一般为两个人)
命名规则：master
```

- dev: parent是master分支，持续集成分支，所有新功能的集成分支，主要的开发分支，会进行频繁的合并更新操作
```
提交权限：无
合并权限：所有开发
命名规则：dev
```

- release: parent是dev分支，预发布分支(测试分支)，该分支仅用于修复bug，避免新增新的功能
```
提交权限：所有开发
合并权限：所有开发，仅能合并bug分支，禁止进行feature分支的合并
命名规则：[sprint] 如:1006
```

- hotfix: parent是master分支的某个tag，紧急修复分支，用于修复线上版本紧急的问题
```
提交权限：所有开发
合并权限：无需合并
命名规则：hotfix-[tag]-[author]-[index] 如：feature-1006-congyang-8888
```

- feature: 开发阶段的临时分支，每个feature应对应一个小功能(即Azure Devops上的一个task)，为了防止合并时冲突问题，开发时长不应该超过2个工作日。
```
提交权限：所有开发
合并权限：所有开发，仅能合并bug分支，禁止进行feature分支的合并
命名规则：feature-[version]-[author]-[index] 如：feature-1006-congyang-8888
注意事项：如果提交的代码无对应的task描述，请自己新建一个task，并简要描述做了什么操作，重构代码 or 修复问题等
```