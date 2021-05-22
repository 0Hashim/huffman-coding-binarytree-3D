# huffman-coding-binarytree-3D
An interactive 3D representation of Huffman coding binary tree
* you can grasp a good understanding with this notebook : [huffman coding](https://colab.research.google.com/drive/15k1SzqZP_G3kDl-UqFl7sqyNyFjotI7A?usp=sharing)


%%html 
<!DOCTYPE html>
<html>
<style>
    .mol-container{
  width: 100%;
  height: 400px;
  position: relative;
    }
</style>
<label for="message">put your message here:</label>
<input type="text" id="message" name="message">
<button onclick="myFunction()">render tree</button>
<br><br>
<div id="container-01" class="mol-container"></div>

<body>
<script src="https://3Dmol.org/build/3Dmol-min.js"></script>
<script>

function frequencyof(l,str){
  let sum =0;
  for(let i=0;i<str.length;i+=1){
    if(str[i]==l){
      sum+=1;
    }
  }
  return sum;
}

class node{

constructor(frequency,letter){
this.letter=letter;
this.frequency=frequency;
this.left = null;
this.right = null;
this.next = null;
this.parented = false;
    }

add(next_node){
  if (this.next == null){
  this.next = next_node;
   }else{
  this.next.add(next_node);
}
}


check(k){
  if((this.next == null) && (this.letter!=k)){
    return 0;
  }else if (this.letter==k){
    return 1;
  }else{
    return this.next.check(k);
  }
}

smallest(small){
        if (small==null){
            if (!this.parented){
                small=this;
                }
            }else if (this.frequency < small.frequency && !(this.parented)){
            small=this;
            }

        if (this.next==null){
            if(small!=null){
              small.parented=true;
              return small;
            }else{
              return null;
            }
        }else{
            return this.next.smallest(small);
            }
}


  start(str2){
        for(let i=0;i<str2.length;i+=1){
          if (!this.check(str2[i])){
            let s=new node(frequencyof(str2[i],str2),str2[i]);
            this.add(s);
          }
        }
     }

 binarytree(){
        let l=this.smallest(null);
        let r=this.smallest(null);
        while (r != null){ 
            let parent = new node(l.frequency+r.frequency,'$');
            parent.left = l;
            parent.right = r;
            this.add( parent );
            l = this.smallest(null);
            r = this.smallest(null);
        }
        l.parented=false;
        return l;
}


}

  let element = $('#container-01');
  let config = { backgroundColor: 'white', disableFog: true };
  let viewer = $3Dmol.createViewer( element, config );
  viewer.translate(0,100);


var str = "huffmmmannn";
//let str="latifa";
let m =new node(frequencyof(str[0],str),str[0]);
m.start(str);
let root = m.binarytree();

//viewer.setViewStyle({style:"outline"});


function tree(n,x_,y_,r){
  viewer.addSphere({ center: {x:x_,y:y_ ,z:0}, radius: 2, color: 0xf9ab00,opacity: 1});
  if (n.left != null){
    viewer.addLabel(n.frequency.toString(), {position: {x:x_-1.5, y:y_, z:0}, backgroundColor: 0xe8710a, backgroundOpacity: 0.8, fontColor: "black",fontSize:15});
    viewer.addArrow({start:{x:x_,y:y_,z:0},end:{x:x_-r,y:y_-6,z:0},radius:0.1});
    viewer.addArrow({start:{x:x_,y:y_,z:0},end:{x:x_+r,y:y_-6,z:0},radius:0.1});
    tree(n.left,x_-r,y_-6,r/2);
    tree(n.right,x_+r,y_-6,r/2);
  }else{
    viewer.addLabel(n.letter+" : "+n.frequency.toString(), {position: {x:x_-2.5, y:y_-2, z:0}, backgroundColor: 0xe8710a, backgroundOpacity: 0.8, fontColor: "black",
    fontSize:15});
  }
}


tree(root,0,0,20);
viewer.spin("y",0.3);
viewer.addLabel("huffman binary tree",{position:{x:0,y:0,z:0},useScreen:true,backgroundColor:0xe8710a,backgroundOpacity: 0.8,fontColor:"black"});
viewer.render();

function again(){
var str = document.getElementById("message").value;
//let str="latifa";
let m =new node(frequencyof(str[0],str),str[0]);
m.start(str);
let root = m.binarytree();

//viewer.setViewStyle({style:"outline"});


function tree(n,x_,y_,r){
  viewer.addSphere({ center: {x:x_,y:y_ ,z:0}, radius: 2, color: 0xf9ab00,opacity: 1});
  if (n.left != null){
    viewer.addLabel(n.frequency.toString(), {position: {x:x_-1.5, y:y_, z:0}, backgroundColor: 0xe8710a, backgroundOpacity: 0.8, fontColor: "black",fontSize:15});
    viewer.addArrow({start:{x:x_,y:y_,z:0},end:{x:x_-r,y:y_-6,z:0},radius:0.1});
    viewer.addArrow({start:{x:x_,y:y_,z:0},end:{x:x_+r,y:y_-6,z:0},radius:0.1});
    tree(n.left,x_-r,y_-6,r/2);
    tree(n.right,x_+r,y_-6,r/2);
  }else{
    viewer.addLabel(n.letter+" : "+n.frequency.toString(), {position: {x:x_-2.5, y:y_-2, z:0}, backgroundColor: 0xe8710a, backgroundOpacity: 0.8, fontColor: "black",
    fontSize:15});
  }
}


tree(root,0,0,20);
viewer.spin("y",0.3);
viewer.addLabel("huffman binary tree",{position:{x:0,y:0,z:0},useScreen:true,backgroundColor:0xe8710a,backgroundOpacity: 0.8,fontColor:"black"});
viewer.render();
}

function myFunction(){
  viewer.clear();
  again();
}


</script>
</body>
</html>

