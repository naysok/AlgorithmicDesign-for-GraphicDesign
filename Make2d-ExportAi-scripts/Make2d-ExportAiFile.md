# Make2d-ExportAiFile  

Make2d と Export を自動化するスクリプト。  

- Rhino.RhinoDoc.ActiveDoc を使った Bake  
- ghpythonlib  
- rs.CurrentView()  
- rs.Command()  

無理やりやってます。 詳細はコードの下に貼っておきます。  

Input は、ListAccess に設定。  


bool を入れる y を Bottom にすると、  
Grasshopper が編集不可の状態になることがある（？）、Toggle で。  
Bottom だと、ファイル書き出しで欠損することもあったので、ちょっとなんかあるときはこれを試すと良さそう。  



```python
import Rhino
import rhinoscriptsyntax as rs
import scriptcontext as sc
import ghpythonlib.components as ghpc



len_brep = ghpc.ListLength(Brep)
len_crv = ghpc.ListLength(Crv)


out_flie = path + name + ".ai"



if y == True:


    sc.doc = Rhino.RhinoDoc.ActiveDoc

    for i in xrange(len_brep):
        sel_brep = ghpc.ListItem(Brep, i)
        sc.doc.Objects.AddBrep(sel_brep)


    for j in xrange(len_crv):
        sel_crv = ghpc.ListItem(Crv, j)
        sc.doc.Objects.AddCurve(sel_crv)


    rs.CurrentView("Perspective")


    rs.Command(
        'SelAll'
        + ' -Make2d DrawingLayout=CurrentView '
        + ' ShowTangentEdges=Yes '
        + ' CreateHiddenLines=No '
        + ' ShowViewRectangle=No '
        + ' MaintainSourceLayers=Yes _Enter ')


    rs.CurrentView("Top")


    rs.Command(
        ' -Export ' + out_flie + ' _Enter'
        + ' SelAll Delete')


    rs.CurrentView("Perspective")

    print "Saved!!"


```


---  

---  

### Rhino.RhinoDoc.ActiveDoc を使った Bake  


ATLV EDUCATIONAL RESOURCE / Grasshopper Python の中にあったのを Attributes の部分を消して使ってます。  

[4. GH Python: Baking and Object Attributes](http://atlv.org/education/ghpython/#4)


---  


### ghpythonlib  

```Python
import ghpythonlib.components as ghpc

sel_brep = ghpc.ListItem(Brep, i)

```

 [ghpythonlib.componentsをつかってコンポーネント機能をpythonからコール](http://am.d-xx.com/2015/06/04/ghpythonlib-components%E3%82%92%E3%81%A4%E3%81%8B%E3%81%A3%E3%81%A6%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88%E6%A9%9F%E8%83%BD%E3%82%92python%E3%81%8B%E3%82%89%E3%82%B3%E3%83%BC/)  


---  


### rs.CurrentView()  

アクティブなビューを取得/変更  

```python
rs.CurrentView() # 取得？
rs.CurrentView("Perspective") # 設定

```

API Docs  
[https://developer.rhino3d.com/api/rhinoscript/view_methods/currentview.htm](https://developer.rhino3d.com/api/rhinoscript/view_methods/currentview.htm)


---  


### rs.Command()  

rs.Command(query) で、Rhino のプロンプトと同じことができる  
ハイフンでオプションの有無を選べる？  

```python
rs.Command("Make2d") # 普通に実行
rs.Command("-Make2d") # オプションを付けて実行

```

RhinoForums  
[Running Rhino commands in Grasshopper Python](https://discourse.mcneel.com/t/running-rhino-commands-in-grasshopper-python/22380)



---  
