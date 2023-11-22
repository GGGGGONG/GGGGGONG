- 👋 Hi, I’m @GGGGGONG
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
GGGGGONG/GGGGGONG is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#import 自己看不需要的可以#批注掉
#invalids, valids  list用于收集检测失败或成功的直播源,已检测的同样的host,不再检测,提高效率!
#filename,newfile路径设置，win和linux肯定不一样
#newfile将是检测后可用的直播源，后缀_ttd，如需自行修改
import time,re,json,requests,random
import os.path
from urllib.parse import urlparse
from pprint import pprint
from lxml import etree
import pandas as pd
 
 
def get_lives_data(filename):
        f=open(filename,'r+')
        r = f.readlines()
        lives_data = [x.strip() for x in r if x.strip() != '']
        # lives_data= list(map(lambda x: x.strip(), r))
        # lives_data=lives_data.remove('')
        f.close()
        return lives_data
 
def test_url(newfile,lives_data):
        # ll是电视直播源的链接列表
        # ll=['http://........','https://.......']
        invalids, valids = [], []
        # 用于检测失败或成功的net,不再检测,提高效率
        #l=lives_data.index('&#127795;电影直播,#genre#')
        with open(newfile, 'a+') as f:
                #for line in lives_data[:]:
                for line in lives_data:
                        if line.find(',http') != -1:
                                name = line.split(',http')[0]
                                urls = 'http' + line.split(',http')[-1]
                                if urls.find('#') != -1:
                                        hrefs = urls.split('#')
                                else:
                                        hrefs = [urls]
 
 
                                if len(hrefs) == 1:
                                        url_parse = urlparse(hrefs[0]).netloc
                                        # print(url_parse,invalids,valids)
                                        if url_parse not in invalids:
                                                # print('url_parse not in invalids')
                                                result = get_parse_href_result(name, hrefs[0], valids, f)
                                                invalids = list(set(invalids + result[0]))
                                                valids = list(set(valids + result[1]))
                                        else:
                                                print(f'[无效] {name} -')
                                # print(f'{hrefs[0]}')
                                else:  # 包含#
                                        content = name + ','
                                        for i in range(len(hrefs)):
                                                url_parse = urlparse(hrefs[i]).netloc
                                                if url_parse not in invalids:
                                                        result2 = \
                                                                get_parse_href_result2(name, hrefs[i], valids, f)
                                                        nvalids = list(set(invalids + result2[0]))
                                                        valids = list(set(valids + result2[1]))
                                                        content += result2[2]
                                        else:
                                                print(f'[无效] {name} -')
                                        # print(f'{hrefs[i]}')
                                        if content[:-1] != name:
                                                f.write(content[:-1] + '\n')
                        else:
                                if line[-7:] == '#genre#':f.write('\n' + line + '\n')
                                else:f.write(line + '\n')
                f.close()
                print(f'\n&#127514;效集合√:\n{invalids}')
                print(f'\n&#127542;效集合X:\n{valids}')
def local_live_check():
        filename = '/storage/emulated/0/TVBoxx//公测版/live_local.txt'
        path = os.path.abspath(filename)
        print(path)
        dir, file = os.path.split(path)
        # dir,file = os.path.split(file_path)
        # print(dir,file)“
        # basename=os.path.basename(filename)
        files = os.path.splitext(file)
        newfile = os.path.join(dir, files[0] + '_ttd' + files[1])
        print(newfile)
        if not os.path.isfile(newfile):
                f = open(newfile, 'w')
                f.close()
        # print(os.path.isfile(newfile))
        lives_data = get_lives_data(filename)
        # print(lives_data)
        test_url(newfile, lives_data)
if __name__ == '__main__':
    local_live_check()
    [{'name': '应用多多家庭版', 'url': 'http://yydsys.top/duo'},
{'name': '应用多多备用', 'url': 'https://cloud.lxweb.cn/f/MoJNuG/xm.json'},
{'name': '阿里精品专线', 'url': 'http://yydsys.top/duo/ali'},
{'name': '饭太硬线路', 'url': 'http://饭太硬.top/tv'},
{'name': '肥猫线路', 'url': 'http://肥猫.love'},
{'name': '蜂蜜线路',
  'url': 'https://ghproxy.com/https://raw.githubusercontent.com/FongMi/CatVodSpider/main/json/config.json'},
{'name': '道长线路', 'url': 'https://pastebin.com/raw/5NHaxyGR'}, {'name': '香雅情线路',
  'url': 'https://raw.gitmirror.com/gaotianliuyun/gao/master/XYQ.json'},
{'name': '小米线路', 'url': 'http://xhww.fun:63/小米/DEMO.json'}, {'name': '菜妮丝线路', 'url': 'https://tvbox.cainisi.cf'},
{'name': '俊佬线路', 'url': 'http://home.jundie.top:81/top98.json'},
{'name': '小雅线路', 'url': 'http://101.34.67.237/config/1'},
{'name': '霜辉月明',
  'url': 'https://ghproxy.com/https://raw.githubusercontent.com/lm317379829/PyramidStore/pyramid/py.json'},
{'name': 'dxawi0线路', 'url': 'https://xhdwc.tk/0'},
{'name': '老刘备线路', 'url': 'https://raw.liucn.cc/box/m.json'}, {'name': '运输车(有跑马灯)', 'url': 'https://weixine.net/ysc.json'},
{'name': '潇洒线路', 'url': 'https://la.kstore.space/download/2863/01.txt'},
{'name': '南风线路',
  'url': 'https://agit.ai/Yoursmile7/TVBox/raw/branch/master/XC.json'},
{'name': '蚂蚁论坛线路', 'url': 'https://la.kstore.space/download/2883/0110.txt'},
{'name': '月光宝盒',
  'url': 'https://raw.gitmirror.com/guot55/YGBH/main/ygbox.json'},
{'name': '欧歌线路',
  'url': 'http://tv.nxog.top/m/111.php?ou=%E6%AC%A7%E6%AD%8C&mz=index2&xl=&jar=index2'},
{'name': '吾爱线路', 'url': 'http://52bsj.vip:98/wuai'},
{'name': '云星日记', 'url': 'http://itvbox.cc/tvbox/云星日记/1.m3u8'},
{'name': '星辰线路', 'url': 'http://8.210.232.168/xc.json'},
{'name': '分享者线路',
  'url': 'https://agit.ai/66666/mao/raw/branch/master/00/000.m3u8'},
{'name': '冰河线路', 'url': 'https://ju.binghe.ga/4.txt'},
{'name': '不良帅线路',
  'url': 'https://notabug.org/qizhen15800/My9394/raw/master/ProfessionalEdition.m3u8'},
{'name': '乱世线路', 'url': 'http://www.dmtv.ml/mao/single.json'},
{'name': '一影视线路',
  'url': 'https://ghproxy.com/https://raw.githubusercontent.com/tv-player/tvbox-line/main/tv/fj.json'},
{'name': '佰欣园线路',
  'url': 'https://ghproxy.com/https://raw.githubusercontent.com/chengxueli818913/maoTV/main/44.txt'},
{'name': '甜蜜线路',
  'url': 'https://ghproxy.com/https://raw.githubusercontent.com/kebedd69/TVbox-interface/main/%E7%94%9C%E8%9C%9C.json'},
{'name': '一木线路',
  'url': 'https://ghproxy.com/https://raw.githubusercontent.com/xianyuyimu/TVBOX-/main/TVBox/%E4%B8%80%E6%9C%A8%E8%87%AA%E7%94%A8.json'},
{'name': 'kvymin线路',
  'url': 'https://agit.ai/kvymin/TV/raw/branch/master/Box.json'},
{'name': 'agit/abc线路', 'url': 'https://agit.ai/n/b/raw/branch/a/b/c.json'},
{'name': 'zzz1线路',
  'url': 'https://agit.ai/mmmgit/tvbox/raw/branch/main/zzz1.json'}, {'name': '&#128142;小马线路', 'url': 'https://szyyds.cn/tv/x.json'},
{'name': '业余线路',
  'url': 'https://raw.gitmirror.com/yydfys/yydf/main/yydf/yydf.json'},
{'name': '荷城茶秀', 'url': 'http://rihou.cc:88/荷城茶秀'},
{'name': '高天流云',
  'url': 'https://raw.gitmirror.com/gaotianliuyun/gao/master/js.json'},
{'name': '胖鸭线路',
  'url': 'https://agit.ai/cyl518/yl/raw/branch/master/ml.json'},
{'name': '元兴线路', 'url': 'https://freed.yuanhsing.cf/TVBox/meowcf.json'},
{'name': '猫爪线路',
  'url': 'https://codeberg.org/AK47/drpy-js/raw/branch/main/js.json'},
{'name': '应用多多',
  'url': 'https://jihulab.com/duomv/apps/-/raw/main/fast.json'},
{'name': '欧歌', 'url': 'http://tv.nxog.top/api.php?mz=xb&id=1&b=欧歌'},
{'name': '云星日记', 'url': 'http://jk.itvbox.cc:66/可视TV/云星日记/仓库/api.json'},
{'name': '吾爱多仓', 'url': 'http://52bsj.vip:98/wuaihouse'}]
