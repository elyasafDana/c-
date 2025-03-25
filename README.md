
#include <iostream>

namespace graph{
using namespace std;
class Node{
    private:
    int name;   
    int index; 
    int* weight;
    Node** son;
    int value; // for the algorithmes
    public:

    Node(){
        weight=NULL;
        son=NULL;
        index=0;
        name=0;
        value=__INT_MAX__;
    }

    void setNode(int length,int id) {
        weight=new int[length];
        son = new Node*[length];
        name=id;

    }

    void addSon( Node* v, int w){
        son[index]=v;
        weight[index]=w;
        index++;
    }
    void addSon( Node* v){
        son[index]=v;
        weight[index]=1;
        index++;
    }

    int getName(){
        return name;
    }

    void delSon(Node* v){
        int delNum= (*v).getName();
        bool found=false;
        for(int i=0;i<index;i++){
            if ((*son)[i].getName()==delNum )
            {
                found=true;
                delete son[i];
                 weight[i]=0;
                while (i<index)
                {
                   son[i]=son[i+1];
                   weight[i]=weight[i+1];
                }
                
            }
            
        }
        if (!found)
        {
            //here we need to add error
        }
    

    }

    void printNode(){
        std::cout<<name;
        for(int i=0;i<index;i++){
            std::cout<<"->" << son[i]->getName()<<"->";
        }
    }


};
    class Graph{
        private:
        Node* arr;
        const int num;
    
        public:
    
        Graph(int x): num(x){

            arr = new Node[x];
            for (int i = 0; i < x; i++)
            {
               arr[i].setNode(x,i);
            }
            
        }  //restart list bacuse of the const

        void add(int v,int u,int w){
            arr[v].addSon(&arr[u],w);
            arr[u].addSon(&arr[v],w);
        }

        void del(int v,int u){
            (arr[v]).delSon(&arr[u]);
            arr[u].delSon(&arr[v]);

        }

        void printGrapgh(){
            for (int i = 0; i <num; i++)
            {
                arr[i].printNode();
                cout<<""<<endl;
            }
            
            

        }
    
    
    };
    
    
    
    }

    namespace ququ{

        class ququ(
        private:
            Node** arr;
            int index;
            int arrSize;
            
        public:

        ququ(Node* n){
            arr=new Node[5];
            index=1;
            arr[0]=n;
        }

        )

    }
    
    int main(){ 
    using namespace graph;
    

    Graph A= Graph(8);
    A.add(5,3,5);
    A.add(0,4,5);
    A.add(4,5,5);
    A.printGrapgh();
    
    }
