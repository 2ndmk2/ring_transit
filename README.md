## 使い方

太陽系外惑星がリングを持っている時のトランジットカーブを書いてくれる。  
コードはcで書かれているので、wrapperでpythonでも使えるようにする。

## 1. 下準備 
-  gcc, pythonなどを使えるようにしておく。  
- GNU Scientific Library (GSL)をcで使えるようにしておく。  
  Homebrewなどを使っているのならば、brew installなどでgslはインストールできる。  
  gslがインストールできたかどうかは、gcc “-lgsl"などのリンカが通るかで確認しておく。  

## 2. wrapperでringのtransit light curveをpythonで使えるようにする
以下の二つのコマンドを(python_extフォルダで)打つと、shared library (c_compile_ring.so)が作成できる。  
ただし、  
/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.8/Headers  
はPython.hがある場所のpathに変更しておくようにする。  

- gcc -fpic -I/Library/Developer/CommandLineTools/Library/Frameworks/Python3.framework/Versions/3.8/Headers -o c_compile_ring.o -c c_compile_ring.c  
- gcc -undefined dynamic_lookup -bundle -lgsl -lgslcblas c_compile_ring.o -o c_compile_ring.so  

## 3. 走らせる
python で作成したc_compile_ring.soをimportすれば使用できる。  
exoring_testフォルダのfit_test.pyに使用例を載せた。  
- python fit_test.py  
と打てば動く。このコードでは、実際にAizawa+2017で解析したデータを読み込み(Q8_l_KIC10403228_test_detrend.dat)  
それと指定したパラメータ (para_result_ring.datの2列目)でのモデルフラックスを並べてplotしてくれる。  

## 番外編 (./c_ext)
c_extフォルダでは実際にcを用いたフィッティングなどを行ってくれる。  
gslをインストールした後に、  
- gcc main.c mpfit.c -lgsl -lgslcblas  
として作成した実行ファイルを実行するとフィティングまでしてくれる。  
