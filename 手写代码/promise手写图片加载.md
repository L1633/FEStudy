### Promise 加载图片
function loadImg(src){
  return new Promise((resolve, reject) => {

    const img = document.createElement('img');

    img.onload = () => {
      resolve(img)
    }

    img.onerror = () => {
      const err = new Error(`图⽚加载失败 ${src}`)
      reject(err)
    }

    img.src = src;
  })
}