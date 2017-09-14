package com.example.lyj;

import java.io.File;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

import android.R.anim;
import android.R.drawable;
import android.media.MediaRecorder;
import android.os.Bundle;
import android.os.Environment;
import android.app.Activity;
import android.app.AlertDialog;
import android.app.Dialog;
import android.content.DialogInterface;
import android.util.Log;
import android.view.ContextMenu;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.ContextMenu.ContextMenuInfo;
import android.view.View.OnClickListener;
import android.view.animation.Animation;
import android.view.animation.AnimationUtils;
import android.widget.AdapterView;
import android.widget.AdapterView.OnItemClickListener;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.ListView;
import android.widget.Toast;

public class MainActivity extends Activity {
	Button start, stop;
	ListView lv;
	// 音频文件  
	File soundFile;
	MediaRecorder mediaRecorder;
	// 定义对话框对象
	private AlertDialog alertDialog;
	private RecordAdapter mAdapter;
	// 刷新按钮
	protected MenuItem refreshItem;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		// 检测SD卡
		if (!U.sdCardExists()) {
			alertDialog = new AlertDialog.Builder(this)
					.setTitle("错误")
					.setMessage("SD卡错误，请检查！")
					.setIcon(R.drawable.ic_launcher)
					.setPositiveButton("确定",
							new DialogInterface.OnClickListener() {
								@Override
								public void onClick(DialogInterface dialog,
										int which) {
									// 退出程序
									System.exit(0);
								}
							}).create();
			alertDialog.show();
		}
		findView();
		listener();
	}

	private void findView() {
		start = (Button) findViewById(R.id.start);
		stop = (Button) findViewById(R.id.stop);
		lv = (ListView) findViewById(R.id.listView1);
	}

	private void listener() {
		// 开始录音
		start.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View arg0) {

				try {
					Log.d("lb", Environment.getExternalStorageDirectory()
							.getCanonicalFile().toString());
					String s = new SimpleDateFormat("yyyyMMdd_hhmmss")
							.format(new Date(System.currentTimeMillis()));
					// 直接存储到了sdcard中
					soundFile = new File(Environment
							.getExternalStorageDirectory().getCanonicalFile()
							+ "/" + s + ".wav");
					mediaRecorder = new MediaRecorder();
					mediaRecorder.setAudioSource(MediaRecorder.AudioSource.MIC); // 录制的声音的来源
					// recorder.setVideoSource(video_source); //录制视频
					// 录制的声音的输出格式（必须在设置声音的编码格式之前设置）
					mediaRecorder
							.setOutputFormat(MediaRecorder.OutputFormat.THREE_GPP);
					// 设置声音的编码格式
					mediaRecorder
							.setAudioEncoder(MediaRecorder.AudioEncoder.DEFAULT);
					// 设置声音的保存位置
					mediaRecorder.setOutputFile(soundFile.getAbsolutePath());
					mediaRecorder.prepare(); // **准备录音**
					mediaRecorder.start(); // **开始录音**
				} catch (IllegalStateException e) {
					e.printStackTrace();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		});
		// 停止录音
		stop.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {
				if (soundFile != null && soundFile.exists()) {
					mediaRecorder.stop(); // **停止录音**
					mediaRecorder.release(); // **释放资源**
					mediaRecorder = null;
				}
			}
		});
	}

	// 更新ListView数据
	private void updateData() {
		File[] files = null;
		if (U.sdCardExists()) {
			File file = new File(U.DATA_DIRECTORY);
			// 查找该目录下所有wav格式文件
			WavFileNameFilter filenameFilter = new WavFileNameFilter(".wav");
			files = file.listFiles(filenameFilter);
		}
		mAdapter = new RecordAdapter(this, files);

		// 设置适配器
		lv.setAdapter(mAdapter);

	}

	@Override
	protected void onResume() {
		super.onResume();
		updateData();
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.main, menu);
		return true;
	}

	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
		switch (item.getItemId()) {
		case R.id.refresh:
			updateData();
			return true;
		default:
			return super.onOptionsItemSelected(item);
		}
	}

}
