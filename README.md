一个任务。
将cloudtohttp函数使用sdk的getTempFileURL进行替换
需修改两个functions：asset->utill->cloudtohttp和asset->index->refreshlist
cloudtohttp()与refreshlist()需要加await

1、cloudtohttp结合原文件和官方文档中对getTempFileURL()接口的描述，做出以下修改：

function cloudtohttp(src) {
    if(src==""){
        return "";
    }
    let result = await app.getTempFileURL({
            fileList: [src]
    }).then(rs=>{
        return rs;
    })
    return result.fileList[0].tempFileURL; // 打印文件访问链接
}

2、refreshlist仅需修改一行，在源文件的134行

if(tempitem.imgs!=null && tempitem.imgs.length!=0){
            let itemimages = document.createElement('div');
            itemimages.setAttribute('class','list-item-images');

            for(let n in tempitem.imgs){
                let img = document.createElement('img');
                img.src = await cloudtohttp(tempitem.imgs[n]);
                img.setAttribute('onclick','previewnetimg("'+img.src+'")');
                itemimages.appendChild(img);
            }
            listitem.appendChild(itemimages);
        }
