graph TD

%%==========================
%% 上流：案件発生〜調達
%%==========================
subgraph UPPER[案件発生〜調達フェーズ]
    START[客先注文計画受付]
    START --> C[工事番号採番]
    C --> REQ_QUOTE[仕入先へ見積もり要求]
    REQ_QUOTE --> D_QUOTE[仕入先見積納期決定]
    D_QUOTE --> B{当社での計画立案}
    B --> E[客先との計画すり合わせ<br>月次計画・合意]
    E --> A[客先からの受注]
    A --> D[調達課から仕入先へ発注]
end

D --> F_JOINT[計画結果の社内共有<br>ガントチャート形式]

subgraph MID_PLAN[計画共有と納期決定]
    F_JOINT --> G[発注-組立課への調達期間決定]
    G --> H[供給納期決定]
end

%%==========================
%% 中流：供給準備〜物流管理
%%==========================
subgraph MID_SUPPLY[供給準備と物流管理]
    H --> I[供給納期-2週間前]
    I --> J[仕分け準備開始]
    J --> K{部品の所在把握}
    K --> M[物流管理<br>未納・未達部品の管理]
end

%%==========================
%% 下流：部品調達〜入荷
%%==========================
D --> Q[仕入先からの部品納品]

subgraph ARRIVAL[部品調達-入荷フェーズ]
    Q --> R[事務所受付-初期判断]
    R --> S{着荷先指示}
    S -->|検査場| T[検査場へ納品]
    S -->|倉庫| U[倉庫へ納品]
    S -->|事務所| V[事務所へ納品]
end

T --> W[検査場-開梱]
U --> X[倉庫-開梱]
V --> Y[事務所-開梱]

%%==========================
%% 情報処理：図面発行
%%==========================
W --> Z1[納品書提出-検査場]
X --> Z2[納品書提出-倉庫]
Y --> Z3[納品書取出-事務所]

Z1 --> Z_INFO_JOINT[事務所-図面発行開始]
Z2 --> Z_INFO_JOINT
Z3 --> Z_INFO_JOINT

Z_INFO_JOINT --> C1_PROCESS[情報処理と図面発行]
C1_PROCESS --> H_SITE_DRAWING_DELIVERY[図面を各着荷場所へ送付]

%%==========================
%% 照合〜受入検査〜TECHS
%%==========================
H_SITE_DRAWING_DELIVERY --> CHECK_PART_DOC{部品と図面の照合}

CHECK_PART_DOC --> F3_ADD_PROC{受入検査<br>数量・外観チェック}
F3_ADD_PROC -->|NG| F3_NG_PROC[不具合対応依頼]
F3_ADD_PROC -->|OK| F3_JUDGE_POST_PROCESS{工程完了確認}

F3_JUDGE_POST_PROCESS --> INSP_STAMP[検査印押印]
INSP_STAMP --> I_DELAY[TECHS入荷実績入力<br>バーコード・数量]
I_DELAY --> J_TECHS_REFLECT[TECHSへ情報反映]

F3_JUDGE_POST_PROCESS -->|後工程必要| F4_QMC_TRANSFER[後工程手配]
F3_JUDGE_POST_PROCESS -->|後工程不要| F5_TEMP_STORAGE[仮置集積]

F5_TEMP_STORAGE --> I
F4_QMC_TRANSFER --> Q

%%==========================
%% 最終供給
%%==========================
M --> Z[組立工程]

%%==========================
%% 強調色
%%==========================
style I_DELAY fill:#f9d71c,stroke:#333,stroke-width:2px,color:#000
style K fill:#f9d71c,stroke:#333,stroke-width:2px,color:#000
style M fill:#ff7f7f,stroke:#333,stroke-width:2px,color:#000
