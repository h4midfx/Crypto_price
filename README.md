/* جدول کریپتو */
    table {
        margin: 0 auto 40px auto;
        border-collapse: collapse;
        width: 90%;
        max-width: 900px;
        background: #fff;
        border-radius: 10px;
        overflow: hidden;
        box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    }
    th, td {
        padding: 15px;
        border-bottom: 1px solid #eee;
        text-align: center;
    }
    th {
        background-color: #007bff;
        color: white;
        font-size: 16px;
    }
    td {
        font-size: 15px;
    }
    tr:hover {
        background-color: #f1f1f1;
    }
    .positive { color: green; font-weight: bold; }
    .negative { color: red; font-weight: bold; }

    /* موبایل */
    @media (max-width: 600px) {
        th, td {
            padding: 10px;
            font-size: 14px;
        }
    }
</style>
