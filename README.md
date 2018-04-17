# PersistantRecyclerAdapter

PersistantRecyclerAdapter is a simple, lite weight android library that persists data across fragment or activity recretion

## How To Use PersistantRecyclerAdapter

###### Step 1. Add the JitPack repository to your build file 

Add it in your root build.gradle at the end of repositories:

```groovy
allprojects {
	repositories {
		...
		maven { url 'https://jitpack.io' }
	}
}
```

###### Step 2. Add the dependency

```groovy
dependencies {
        compile 'com.github.devmike01:PersistantRecyclerAdapter:-SNAPSHOT'
}
``` 
   
## Example usage
- Create an empty Java and extends `PersistentRecyclerAdapter<SampleItems, SampleAdapter.SampleViewHolder>`. Implements all the neccessary methods
- Create a subclass/class that extends `RecyclerView.ViewHolder`
- Initilized a List variable e.g private `List<SampleItems> itemsList`;
At the end your Adapter class look like the code snippet below:
 
 ```java
 public class SampleAdapter extends PersistentRecyclerAdapter<SampleItems, SampleAdapter.SampleViewHolder> {
        
	private List<SampleItems> itemsList; //Initialize a list with your data model
	
        public SampleAdapter(List<SampleItems> serializedList) {
            super(serializedList);
            itemsList = serializedList;
        }
	
	@Override
        public void onBindViewHolder(@NonNull SampleViewHolder holder, int position, SampleItems data) {
            holder.tv.setText(data.getStr());
        }
	
	@NonNull
        @Override
        public SampleViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
            View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.grid_items_sample, parent, false);
            return new SampleViewHolder(view);
        }
	
    	@Override
        public int getItemCount() {
            return itemsList.size();
        }
	
	//Recycler ViewHolder
	public class SampleViewHolder extends RecyclerView.ViewHolder{
    		TextView tv;
            	public SampleViewHolder(View itemView) {
                	super(itemView);
                	tv = itemView.findViewById(R.id.grid_text);
            }
        }
    }


 ```
 
- Now create a model class and implements Parcelable, like this:

```java
public class SampleItems implements Parcelable{
    private String str;public SampleItems() {
    }

  public static final Creator<SampleItems> CREATOR = new Creator<SampleItems>() {
  
        @Override
        public SampleItems createFromParcel(Parcel in) {
            SampleItems items = new SampleItems();
            items.str = in.readString();
            return items;
        }

  	@Override
        public SampleItems[] newArray(int size) {
            return new SampleItems[size];
        	}
    	};
	
	public void setStr(String s){
        	str =s;
    	}
	public String getStr(){
        	return str;
    	}
	
	@Override
    	public int describeContents() {
        	return 0;
    	}
	
    	@Override
    	public void writeToParcel(Parcel dest, int flags) {
        	dest.writeString(str);
    	}
}
```

- We can now intilize the adapter in our Fragment or Activity class and pass the network data we want to display to it. Please note that PersistantRecyclerAdapter only supports two **LayoutManager**, and they're listed below:

* LinearLayoutManager
```java
LinearLayoutManager mLinearLayout = a.getLinearLayoutManager(getActivity(), 3);
 mLinearLayout.setLayoutManager(gridLayout);
 mLinearLayout.setHasFixedSize(true);
 mLinearLayout.setAdapter(a);
 ```
* GridLayoutManager
```java
GridLayoutManager gridLayout = a.getGridLayoutManager(getActivity(), 3);
 gridView.setLayoutManager(gridLayout);
 gridView.setHasFixedSize(true);
 gridView.setAdapter(a);
 ```
**StaggeredLayoutManager** would be added in the future. 

## Contributions

If you would like to contribute to PersistantRecyclerAdapter for Android, clone the repository and just make a pull request.

git clone https://github.com/devmike01/PersistantRecyclerAdapter.git


## License

Copyright 2018 Gbenga Oladipupo

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and limitations under the License.