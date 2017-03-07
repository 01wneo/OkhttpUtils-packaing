# OkhttpUtils-packaing
��okhttputils�ķ�װ�࣬okhttputils����[https://github.com/hongyangAndroid/okhttputils.


## �÷�

* Android Studio
	
	```
	compile 'com.zhy:okhttputils:2.6.2'
	```
	��ǰʹ�õ�������汾,�����汾δ����
	

## ��һ����������Ŀ�´���һ���ӿڼ̳�`lib.network.bean`���µ�`NetworkListener`�ӿ�,����ӿ����ƿ�����ȡ,���ϱ��˵Ĵ���:

```java
public interface INetwork extends NetworkListener {

    /**
     * @param tag
     * @param request
     */
    void exeNetworkRequest(int tag, NetworkRequest request);

    /**
     * ���������������Դ�������ʱʱ��, ������һЩ��Ҫ�������Ե�����
     *
     * @param tag
     * @param request
     * @param listener
     */
    void exeNetworkRequest(int tag, NetworkRequest request, NetworkListener listener);

    /**
     * ȡ��������������
     */
    void cancelAllNetworkRequest();

    void cancelNetworkRequest(int tag);
}
```


## �ڶ���������Ŀ��ײ��Fragment��Activity��ʵ��`INetwork`����㶨��Ľӿ�,��������������Ĵ���,
    ����һ��`lib.network`���µ�`NetworkExecutor`�ĳ�Ա����,
	```java
	private NetworkExecutor mNetworkExecutor;
```

	Ȼ��,
```java
 public void exeNetworkRequest(int what, NetworkRequest request) {
        exeNetworkRequest(what, request, this);
    }

    public void exeNetworkRequest(int what, NetworkRequest request, NetworkListener listener) {
        if (isFinishing()) {
            return;
        }
        if (mNetworkExecutor == null) {
            mNetworkExecutor = new NetworkExecutor(getClass().getName(), this);
        }
        mNetworkExecutor.execute(what, request, listener);
    }

    @Override
    public void cancelAllNetworkRequest() {
        if (mNetworkExecutor != null) {
            mNetworkExecutor.cancelAll();
        }
    }

    @Override
    public void cancelNetworkRequest(int tag) {
        if (mNetworkExecutor != null) {
            mNetworkExecutor.cancel(tag);
        }
    }
	
```
	��Ҫ������`onDestroy`���������ȡ������
		```java
    @Override
    protected void onDestroy() {
        super.onDestroy();

        if (mNetworkExecutor != null) {
            mNetworkExecutor.destroy();
            mNetworkExecutor = null;
        }
    }
	
```

## �������ͼ���,���㴴����Activity������Fragment�̳���д�Ļ���ʱ,
	��������͵�����������Ϳ�����,
			```java
	exeNetworkRequest(int what, NetworkRequest request);
```
	��һ������what�����㿪�������������,����ɹ�����ʧ�ܻ�����ȡ�������õ���ֵ,
	�ڶ������������������ʵ��,NetworkRequest����ͨ��`lib.network.bean.NetworkRequest`�������
				```java
		NetworkRequest task = newPost(String url);
```
�ȷ����õ�,Ҳ����ͨ��`lib.network.bean.NetworkRequest`�������
				```java
		task.addParam(String name, String value);
```
�ȷ����������Ĳ���

## �������������Ľ���:
			```java
	    Object onNetworkResponse(int id, NetworkResponse nr);
```
����ͨ��`nr.getText()`�õ����󵽵���������(Sring�ַ�������),�������������ѵõ���String�������ݽ�����json���ݲ���װ��bean���

			```java
	     void onNetworkSuccess(int id, Object result);
```
�������������Գɹ��õ��������ݲ������ɹ���Ĳ�����,`result`�����������

			```java
	     void onNetworkError(int id, NetError error);
```
��������������������ݴ��������������ʱ�����ķ���,����ȡ��Loading��,���Դ�`error`���õ���������



## ����

```�ÿ��
 -keep class okhttp3.**
 -keep class okio.**

#okhttp��
-dontwarn okhttp3.**
-keep class okhttp3.**{*;}


#okio��
-dontwarn okio.**
-keep class okio.**{*;}


```






