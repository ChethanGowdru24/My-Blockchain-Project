# 🔗 Simple Blockchain Project

This is a basic blockchain implementation built using Python and Flask.

## 🚀 Features
- Create and mine new blocks
- Add transactions
- Validate blockchain
- Simple REST API

## 🛠️ Tech Used
- Python
- Flask

## ▶️ How to Run
1. Install dependencies:
   pip install -r requirements.txt

2. Run the app:
   python main.py

3. Open in browser:
   http://127.0.0.1:5000/get_chain

## 📌 API Endpoints
- /mine_block → Mine a new block
- /get_chain → View blockchain
- /add_transaction → Add transaction
- /is_valid → Check validity

## 📷 Output Example
(Add screenshots here later)

## 📄 License
This project is open-source under the MIT License.
from flask import Flask, jsonify, request
from blockchain import Blockchain

app = Flask(name)

blockchain = Blockchain()

@app.route('/mine_block', methods=['GET'])
def mine_block():
    previous_block = blockchain.get_previous_block()
    proof = blockchain.proof_of_work(previous_block['proof'])
    previous_hash = blockchain.hash(previous_block)

    blockchain.add_transaction(sender="system", receiver="you", amount=1)

    block = blockchain.create_block(proof, previous_hash)

    return jsonify({
        'message': 'Block mined!',
        'block': block
    }), 200

@app.route('/get_chain', methods=['GET'])
def get_chain():
    return jsonify({
        'chain': blockchain.chain,
        'length': len(blockchain.chain)
    }), 200

@app.route('/add_transaction', methods=['POST'])
def add_transaction():
    json_data = request.get_json()
    required = ['sender', 'receiver', 'amount']

    if not all(k in json_data for k in required):
        return 'Missing values', 400

    index = blockchain.add_transaction(
        json_data['sender'],
        json_data['receiver'],
        json_data['amount']
    )

    return jsonify({'message': f'Transaction will be added to Block {index}'}), 201

@app.route('/is_valid', methods=['GET'])
def is_valid():
    valid = blockchain.is_chain_valid(blockchain.chain)

    return jsonify({
        'message': 'Valid blockchain' if valid else 'Invalid blockchain'
    }), 200

if name == 'main':
    app.run(debug=True)

# Simple Blockchain Project

This is a basic blockchain implementation using Python and Flask.

## Features
- Mine blocks
- Add transactions
- Validate chain

## Run

pip install -r requirements.txt
python main.py

