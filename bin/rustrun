BASEDIR=$(pwd)
if [ ! $# -eq 1 ]; then
    echo "There should be 1 and only 1 argument"
    exit 1
fi
FILE=$1
if [ ! -f "$FILE" ]; then
    echo "$FILE does not exist"
    exit 1
fi
cd /tmp
TEMP_DIR_NAME=$$
CARGONAME="rustrun${TEMP_DIR_NAME}"
cargo new --bin $CARGONAME > /dev/null 2>&1
cd $CARGONAME
cp $BASEDIR/$1 ./src/main.rs
L=`grep '^use ' ./src/main.rs | sed 's/^....//' | sed 's/::.*//' | awk '!x[$0]++'`
for n in $L
do
    if [ "$n" != "std" ]; then
        echo "$n = \"*\"" >> Cargo.toml
    fi
done
cargo +nightly build > /dev/null 2>&1
./target/debug/$CARGONAME
cd ..
rm -rf $CARGONAME
cd $BASEDIR