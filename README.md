
#include <iostream>

namespace graph{

    class Graph{
        private:
            const int num;
    
        public:
    
        Graph(int x): num(x){}  //restart list bacuse of the const
    
    
    };
    
    class Node{
        private:
        int name;
        int index; 
        int* weight;
        Node** son;
        public:
        Node(int length,int id){
            weight=new int[length];
            son = new Node*[length];
            index=0;
            name=id;
        }
    
        void addSon( Node* v, int w){
            son[index]=v;
            weight[index]=w;
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
                    while (i<index)
                    {
                       son[i]=son[i+1];
                    }
                    
                }
                
            }
            if (!found)
            {
                //here we need to add error
            }
        
    
        }
    
        void print (){
            std::cout<<name;
            for(int i=0;i<index;i++){
                std::cout<<son[i]->getName();
            }
        }
    
    
    };
    
    }
    
    int main(){ using namespace graph;
    
    Node a= Node(3,3);
    Node b= Node(4,4);
    
    a.addSon(&b,4);
    a.print();
    
    }
