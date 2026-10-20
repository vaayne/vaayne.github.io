# 我的 Agent 工作流：配图源文件

博客 content/posts/2026-10-My-Agent-Workflow.md 的配图。用 documd 导出 PNG：

    npx @markdown-viewer/documd figures/2026-10-my-agent-workflow.md --assets static/images/2026/10 --kind diagrams

图 1 回答：Agent、BB 和 Magpie 分别跑在哪台机器上。

<div style="max-width: 820px; box-sizing: border-box; position: relative;">
  <style scoped>
    .wf-doc { background: #f8fafc; padding: 22px; font-family: 'PingFang SC', 'Segoe UI', sans-serif; color: #1f2937; }
    .wf-title { margin: 0 0 4px; text-align: center; font-size: 22px; font-weight: 700; }
    .wf-sub { margin: 0 0 16px; text-align: center; font-size: 12px; color: #676f7e; }
    .wf-band { padding: 10px 12px; border-radius: 6px; background: #ffffff; border: 1px dashed #5b6b8c; margin-bottom: 10px; }
    .wf-band-head { font-size: 11px; font-weight: 700; letter-spacing: 0.08em; color: #676f7e; margin-bottom: 8px; }
    .wf-grid { display: grid; gap: 8px; }
    .wf-g2 { grid-template-columns: repeat(2, 1fr); }
    .wf-g3 { grid-template-columns: repeat(3, 1fr); }
    .wf-chip { padding: 7px 9px; font-size: 12px; text-align: center; background: #f8fafc; border: 1px solid #5b6b8c; border-radius: 3px; }
    .wf-link { padding: 8px 12px; margin-bottom: 10px; text-align: center; font-size: 12px; border-radius: 6px; background: #dfe5fb; border: 1px solid #265aac; }
    .wf-link b { font-weight: 700; }
    .wf-cluster { padding: 12px; border-radius: 6px; background: #eef2fb; border: 1px solid #5b6b8c; margin-bottom: 10px; }
    .wf-cluster-head { display: flex; align-items: baseline; gap: 8px; margin-bottom: 10px; }
    .wf-cluster-name { font-size: 12px; font-weight: 700; letter-spacing: 0.08em; }
    .wf-cluster-meta { font-size: 11px; color: #676f7e; }
    .wf-machines { display: grid; grid-template-columns: 1.4fr 1fr 1fr; gap: 8px; align-items: start; }
    .wf-machine { padding: 10px; border-radius: 4px; background: #ffffff; border: 1px solid #5b6b8c; }
    .wf-machine.hub { border: 2px solid #265aac; }
    .wf-machine-head { font-size: 13px; font-weight: 700; text-align: center; }
    .wf-machine-role { font-size: 11px; color: #676f7e; text-align: center; margin-bottom: 8px; }
    .wf-stack { display: grid; gap: 5px; }
    .wf-node { padding: 6px 8px; font-size: 11px; line-height: 1.3; background: #eef2fb; border: 1px solid #5b6b8c; border-radius: 3px; text-align: center; }
    .wf-node.primary { background: #d9e3f4; border-color: #265aac; font-weight: 600; }
    .wf-node small { display: block; font-size: 10px; color: #676f7e; font-weight: 400; }
    .wf-note { margin: 10px 0 0; font-size: 11px; color: #676f7e; text-align: center; }
  </style>
  <section class="wf-doc">
    <h1 class="wf-title">我的 Agent 工作流</h1>
    <p class="wf-sub">三台机器，一个 BB；订阅只在 MacMini 上配置一次</p>
    <div class="wf-band">
      <div class="wf-band-head">我在任何地方</div>
      <div class="wf-grid wf-g2">
        <div class="wf-chip">BB 手机 App</div>
        <div class="wf-chip">任意浏览器</div>
      </div>
    </div>
    <div class="wf-link"><b>bb connect</b> · https://&lt;handle&gt;.getbb.app · 不用内网穿透（也可以走 Tailscale）</div>
    <div class="wf-cluster">
      <div class="wf-cluster-head"><span class="wf-cluster-name">BB 集群</span><span class="wf-cluster-meta">所有机器都连到 MacMini 上的 BB Server</span></div>
      <div class="wf-machines">
        <div class="wf-machine hub">
          <div class="wf-machine-head">MacMini</div>
          <div class="wf-machine-role">Homelab · 常年开机</div>
          <div class="wf-stack">
            <div class="wf-node primary">BB Server<small>线程 · 编排 · Web UI</small></div>
            <div class="wf-node primary">Magpie<small>所有订阅账号和 API Key</small></div>
            <div class="wf-node">bb magpie 扩展<small>转发各机器的模型请求</small></div>
            <div class="wf-node">Pi · Codex · Claude Code</div>
          </div>
        </div>
        <div class="wf-machine">
          <div class="wf-machine-head">MacBook</div>
          <div class="wf-machine-role">主力工作设备</div>
          <div class="wf-stack">
            <div class="wf-node">Pi · Codex · Claude Code<small>无需配置 Provider</small></div>
          </div>
        </div>
        <div class="wf-machine">
          <div class="wf-machine-head">VPS</div>
          <div class="wf-machine-role">线上机器 · 随时可用</div>
          <div class="wf-stack">
            <div class="wf-node">Pi · Codex · Claude Code<small>无需配置 Provider</small></div>
          </div>
        </div>
      </div>
    </div>
    <div class="wf-band">
      <div class="wf-band-head">模型提供方 · 只由 Magpie 连接</div>
      <div class="wf-grid wf-g3">
        <div class="wf-chip">Claude 订阅</div>
        <div class="wf-chip">ChatGPT / Codex 订阅</div>
        <div class="wf-chip">其他 API Key</div>
      </div>
    </div>
    <p class="wf-note">实线 = 我的机器；虚线 = 外部。新机器加入 BB 集群后，模型请求自动经 BB 转发到 Magpie。</p>
  </section>
</div>

图 2 回答：2026-10-16 这一天，线程在什么时间、哪台机器上运行。数据来自 BB 线程记录，已匿名。

```echarts
{
 "width": 760,
 "textStyle": {"fontFamily": "PingFang SC, Segoe UI, sans-serif"},
 "height": 300,
 "title": {
  "text": "2026-10-16：78 个线程，76 个由 Agent 派出",
  "subtext": "每个点是一个线程；横轴是创建时间，纵轴是运行的机器",
  "left": 14,
  "top": 8,
  "textStyle": {
   "color": "#1f2937",
   "fontSize": 15
  },
  "subtextStyle": {
   "color": "#676f7e",
   "fontSize": 11
  }
 },
 "color": [
  "#2b66c4",
  "#2f9e44",
  "#f3a33c",
  "#d1242f",
  "#7048e8",
  "#0f9b9b",
  "#c16f8a",
  "#7c5a3d"
 ],
 "legend": {
  "top": 14,
  "right": 20,
  "itemWidth": 10,
  "itemHeight": 10,
  "textStyle": {
   "color": "#1f2937",
   "fontSize": 11
  }
 },
 "grid": {
  "left": 80,
  "right": 24,
  "top": 70,
  "bottom": 56
 },
 "xAxis": {
  "type": "value",
  "min": 0,
  "max": 24,
  "interval": 3,
  "axisLabel": {
   "color": "#676f7e",
   "formatter": "{value}:00"
  },
  "axisLine": {
   "lineStyle": {
    "color": "#5b6b8c"
   }
  },
  "splitLine": {
   "lineStyle": {
    "color": "#d9e3f4"
   }
  }
 },
 "yAxis": {
  "type": "category",
  "data": [
   "VPS",
   "MacBook",
   "MacMini"
  ],
  "axisLabel": {
   "color": "#1f2937",
   "fontSize": 12
  },
  "axisLine": {
   "lineStyle": {
    "color": "#5b6b8c"
   }
  },
  "splitLine": {
   "show": true,
   "lineStyle": {
    "color": "#d9e3f4"
   }
  }
 },
 "series": [
  {
   "name": "Codex",
   "type": "scatter",
   "symbolSize": 11,
   "itemStyle": {
    "color": "#2b66c4",
    "opacity": 0.75,
    "borderColor": "#ffffff",
    "borderWidth": 1
   },
   "data": [
    [
     0.03,
     "MacMini"
    ],
    [
     0.12,
     "MacBook"
    ],
    [
     0.18,
     "MacMini"
    ],
    [
     0.18,
     "MacMini"
    ],
    [
     0.28,
     "MacMini"
    ],
    [
     0.32,
     "MacMini"
    ],
    [
     0.33,
     "MacMini"
    ],
    [
     0.37,
     "MacBook"
    ],
    [
     0.45,
     "MacMini"
    ],
    [
     0.52,
     "MacBook"
    ],
    [
     0.53,
     "MacMini"
    ],
    [
     0.65,
     "MacBook"
    ],
    [
     0.68,
     "MacMini"
    ],
    [
     0.92,
     "MacMini"
    ],
    [
     1.2,
     "MacMini"
    ],
    [
     1.27,
     "MacMini"
    ],
    [
     1.52,
     "MacMini"
    ],
    [
     1.53,
     "MacMini"
    ],
    [
     1.6,
     "MacMini"
    ],
    [
     1.75,
     "MacMini"
    ],
    [
     1.93,
     "MacMini"
    ],
    [
     1.95,
     "MacMini"
    ],
    [
     2.12,
     "MacMini"
    ],
    [
     2.32,
     "MacMini"
    ],
    [
     2.32,
     "MacMini"
    ],
    [
     2.4,
     "MacMini"
    ],
    [
     2.6,
     "MacMini"
    ],
    [
     2.82,
     "MacMini"
    ],
    [
     2.87,
     "MacMini"
    ],
    [
     3.13,
     "MacMini"
    ],
    [
     3.22,
     "MacMini"
    ],
    [
     8.95,
     "MacMini"
    ],
    [
     10.32,
     "MacMini"
    ],
    [
     10.32,
     "MacMini"
    ],
    [
     10.53,
     "MacMini"
    ],
    [
     10.92,
     "MacMini"
    ],
    [
     10.98,
     "MacMini"
    ],
    [
     12.33,
     "MacMini"
    ],
    [
     13.88,
     "MacMini"
    ],
    [
     16.28,
     "MacMini"
    ],
    [
     16.58,
     "MacMini"
    ],
    [
     16.92,
     "MacBook"
    ],
    [
     19.5,
     "MacMini"
    ],
    [
     19.65,
     "MacMini"
    ],
    [
     19.75,
     "MacBook"
    ],
    [
     21.67,
     "MacMini"
    ],
    [
     22.53,
     "MacMini"
    ],
    [
     22.65,
     "MacMini"
    ],
    [
     23.37,
     "MacMini"
    ]
   ],
   "markArea": {
    "silent": true,
    "itemStyle": {
     "color": "#eef2fb"
    },
    "label": {
     "show": true,
     "position": "insideTop",
     "color": "#1f2937",
     "fontSize": 12,
     "formatter": "我在睡觉"
    },
    "data": [
     [
      {
       "xAxis": 0
      },
      {
       "xAxis": 3.25
      }
     ]
    ]
   }
  },
  {
   "name": "Claude Code",
   "type": "scatter",
   "symbolSize": 11,
   "itemStyle": {
    "color": "#2f9e44",
    "opacity": 0.75,
    "borderColor": "#ffffff",
    "borderWidth": 1
   },
   "data": [
    [
     8.62,
     "MacMini"
    ],
    [
     9.42,
     "MacMini"
    ],
    [
     10.7,
     "MacMini"
    ],
    [
     10.75,
     "MacMini"
    ],
    [
     10.8,
     "MacBook"
    ],
    [
     11.25,
     "MacMini"
    ],
    [
     11.3,
     "VPS"
    ],
    [
     11.68,
     "MacBook"
    ],
    [
     11.8,
     "MacMini"
    ],
    [
     12.18,
     "MacMini"
    ],
    [
     12.57,
     "MacMini"
    ],
    [
     13.27,
     "MacBook"
    ],
    [
     13.35,
     "MacMini"
    ],
    [
     13.43,
     "MacMini"
    ],
    [
     13.6,
     "MacMini"
    ],
    [
     13.78,
     "MacMini"
    ],
    [
     13.98,
     "MacMini"
    ],
    [
     14.37,
     "MacMini"
    ],
    [
     14.6,
     "MacMini"
    ],
    [
     14.9,
     "MacMini"
    ],
    [
     15.17,
     "MacMini"
    ],
    [
     16.28,
     "MacMini"
    ],
    [
     16.35,
     "MacBook"
    ],
    [
     16.67,
     "MacMini"
    ],
    [
     16.92,
     "MacBook"
    ],
    [
     17.08,
     "MacMini"
    ],
    [
     21.42,
     "MacMini"
    ]
   ]
  },
  {
   "name": "Pi",
   "type": "scatter",
   "symbolSize": 11,
   "itemStyle": {
    "color": "#f3a33c",
    "opacity": 0.75,
    "borderColor": "#ffffff",
    "borderWidth": 1
   },
   "data": [
    [
     8.57,
     "MacMini"
    ],
    [
     11.22,
     "VPS"
    ]
   ]
  }
 ]
}
```
