## 1.git
��װbrew��mac��
mac http://brew.sh
```
brew install git 
```
### 1.1 ����git����˭
```
git config --global user.name xxxxx
git config --global user.email xxxxx
git config --list
```
### 1.2 ��ʼ��git
```
git init
```

### ����������Ŀ¼ 
```
mkdir gitTest && cd gitTest
```

### ls��ʾ�����ļ�
```
ls -al ��ʾ���ص�
```

### �����ļ�
```
touch index.txt
cat index.txt �鿴����
```

### �鿴git״̬
```
git status 
```

### ��ӵ��ݴ���
```
git add .
```

### �ύ����ʷ��
```
git commit -m 'write hello world'
```

### �Աȴ���
���������ݴ���
```
git diff
```
�ݴ�������ʷ�� 
```
git diff --cached
```
����������ʷ�� 
```
git diff master(��֧��)
```

## �鿴��־
```
git log �������ʹ�����¼��鿴�����뿴����q�˳�
```

## �鿴������־
```
git reflog
```
## �ع�
```
git reset --hard �汾��
```
## vi�༭
```
vi index.txt
i �༭ģʽ
esc + :wq ���沢�˳�  ǿ���˳�q!
```


## ��֧����
- �鿴��֧
```
git branch 
```
- ������֧
```
git branch <branchName>
```
- �л���֧
```
git checkout <branchName>
```
- ɾ����֧�������Լ�ɾ���Լ���
```

git branch -D <branchName>
```
- �������л�(�൱�ڽ���ǰ���ݿ�¡һ��)
```
git checkout -b <branchName>
```
- ��master�Ϻϲ�dev��֧
```
git merge dev
```
- ������ͻ(�ֶ�����ύ�µİ汾)

## �����ֿ�
- �����ղֿ⣬дһ���ֿ���
```
new repository
```

- ����Ҫ��һ��README.md .gitignore
```
echo '.idea' >> .gitignore
echo 'welcome' >> README.md
```

- �ύ�������ֿ�
```
git add .
git commit -m ''
git remote add origin ��ַ
git remote -v �鿴���й���
git remote rm origin ɾ��
```
- ���͵�Զ�ֿ̲���
```
git push origin master(��������������֧) -u(upstream �´��ύ����������origin master)
```

## ������̬ҳ
- �����ľ�̬ҳ������gh-pages��֧
```
git checkout -b gh-pages
git add .
git commit -m 
git push origin gh-pages
```

## ��ʦ��һ���ֿ� 
- �鳤fork�ҵĲֿ�
- ����Ϣ �ŵ��Լ����ļ������ύ����
- ���͵�github��
- ����pull request����ϲ�

## �鳤����Ա��ͨȨ��
- ��Աclone���´���
- �����Լ����ļ��ύ
- ��ȡ�鳤�Ĳֿ�����´���
```
git pull 
```
- �ٴ�����

## �鳤���͸���ʦ
- ����ȡ�Լ�������´���
```
git pull 
```
- ��ȡ��ʦ�����´���
```
git remote add teacher https://github.com/zhufengzhufeng/201615node.git
```
- ���͵��Լ��Ĳֿ���
```
git push origin master
```
- pull request

## ���̺߳Ͷ��߳�
- node���߳��ǵ��̵߳ģ������а����̣߳�����java һ�������а�������̣߳�node��һ������ֻ�ܰ���һ���̣߳������ӽ��̡�

## ͬ�����첽
- ������ϵ���ִ�У�����ͬ�������첽���첽�����������߳�
- �����һ��Է�

## �����ͷ�����
- ����ں���˵�ģ����������첽��ǰ������

## �ص�����ͷ�ٵ���
- �ûص�������첽�������

## �¼���

## �첽���ļ���д��callback,��ʱ���������첽 ����ͬ��

## nodeȫ�ֶ���
- ������ص����ֱ�ӷ��ʵ�
- ��global�Ϲ��صĶ���ȫ�ֶ���

## js��ģ�� 
- (seajs cmd,requirejs amd,node commonjs)
- cmd �ͽ�����,amd ����ǰ��
- ����(���ܱ�֤��ȫ�����ͻ,����ʱ�������ֹ���)
- �հ���node��ʵ��ģ�黯 ���õ��Ƕ�д

## commonjs(����˿�ά���ԣ������ڷֹ�Э�������ھ۵����)
- ��ζ���ģ��    
����һ��js�ļ���ÿ���ļ�����һ��ģ�飬���ģ��������һ����
- ��ε���һ��ģ��  
exports/module.exports  
- ���ʹ��һ��ģ��  
require

## npm node-package-manager
### ȫ������(����������ʹ��) -g
### ��װnrmԴ�л�����
```
npm i nrm -g
```
### �������Դ
```
nrm ls
nrm add zf http://172.18.1.139
nrm use zf
```
### ��װȫ��
https://github.com/ksky521/nodePPT
```
npm install nodeppt -g
```
### http-server
```
npm install http-server -g
```
��������
```
http-server -p 3000 ���Ķ˿�
```
### ж��
```
npm uninstall http-server -g
```

### ���ذ�װ(�����Ǵ�����ʹ�õ�)
#### ��ʼ�������ļ�(package.json)
��ָ��Ŀ¼������
```
npm init -y
```
#### 1.�������� gulp

```
npm install gulp (--save-dev)����(-D)
```

#### 2.��Ŀ���� jquery
�鿴�汾
```
npm info jquery
npm install jquery@2.2.4 (--save)����(-S)
```

#### 3.�����Լ��İ�
- package.json 
    - name���ܺ��ѷ����İ�����
    - main���Ӧ�����ļ���дһ��
- ����Ҫ�л���npm  
- ����û� �еĻ����Ե�¼
```
npm addUser
```
- ����
```
npm publish 
```
- ж�ذ�
```
npm unpublish  --force
```

## ģ��Ĳ��һ���

> Ϊʲô-g��װ�Ŀ�������������ʹ��,npm��������������ʹ�ã����е�ȫ�ְ� ����װ��npm�ϣ�����npm�´���һ���ű��ļ�������ӳ�䵽��ʵ���ļ��ϣ�����ͨ��ȫ�ְ�װ�Ŀ���ֱ������������ʹ��

## bower(����ǰ���ļ���) 
npm(����nodeģ���) ->��װ���ļ��ŵ�node_modules�� ����ָ����װĿ¼
bower(����ǰ���ļ���) ->�ƶ�Ŀ¼���� ֻ����ǰ���ļ�(��git������)
```
npm install bower -g
```
### 1.1��ʼ�� bower.json��¼����
```
bower init
```
### 1.2�����ļ�
```
bower install bootstrap --save
```
### 1.3 ָ��Ŀ¼
```
.bowerrc
{"directory":"src/public/lic"}
```


> Ĭ�ϰ�װ��bower_component��

### ����hexo
```
npm install hexo-cli -g
```

### ����blog
```
hexo init
```

### ��������
```
hexo server
```

### ���·���_posts��
md��ʽ�ļ�

### �ֿ���
```
github��.github.io
```

### ������git����Ҫ����һ����� 
```
npm install hexo-deployer-git --save
```

### ����_config.yml
```
deploy:
  type: git
  repo: https://username:password@github.com/zuyuan/zuyuan.github.io.git
  branch: master
  message: push
```

## ��������ύ
### ����
```
hexo g
```

### ����
```
hexo deploy
```

### ������ַ
```
zuyuan.github.io
```